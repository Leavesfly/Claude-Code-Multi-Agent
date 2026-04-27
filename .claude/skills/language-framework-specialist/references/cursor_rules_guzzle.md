---
description: Definitive guidelines for using Guzzle HTTP client in PHP, focusing on robust client configuration, effective middleware, PSR-18 interoperability, and testability.
---

# Guzzle 最佳实践

Guzzle 是事实上的 PHP HTTP 客户端。使用这些规则构建健壮、可测试且可维护的 HTTP 集成。

## 1. 客户端实例化和配置

始终将不可变的 Guzzle 客户端作为服务注入。使用全局默认值（尤其是超时）配置一次。

### ✅ 好：具有基本默认值的面向服务的客户端

```php
<?php

use GuzzleHttp\Client;
use GuzzleHttp\HandlerStack;
use GuzzleHttp\Middleware;
use Psr\Log\LoggerInterface;
use Psr\Http\Message\RequestInterface;
use Psr\Http\Message\ResponseInterface;
use GuzzleHttp\Exception\ConnectException;
use Exception;

final class MyApiClientFactory
{
    public static function create(LoggerInterface $logger): Client
    {
        $stack = HandlerStack::create();

        // 1. Logging: Log all requests and responses
        $stack->push(Middleware::log(
            $logger,
            new \GuzzleHttp\MessageFormatter(
                '{method} {uri} HTTP/{version} {code} {res_header_Content-Length}'
            )
        ));

        // 2. Retry Logic: Robust handling for transient errors (5xx, connection issues)
        $stack->push(Middleware::retry(
            function (
                $retries,
                RequestInterface $request,
                ResponseInterface $response = null,
                Exception $exception = null
            ) {
                // Limit to 3 retries
                if ($retries >= 3) {
                    return false;
                }
                // Retry on connection exceptions
                if ($exception instanceof ConnectException) {
                    return true;
                }
                // Retry on server errors (5xx)
                if ($response && $response->getStatusCode() >= 500) {
                    return true;
                }
                return false;
            },
            // Exponential backoff: 100ms, 200ms, 400ms
            function ($retries) {
                return 100 * (2 ** ($retries - 1));
            }
        ));

        return new Client([
            'base_uri' => 'https://api.example.com/v1/',
            'timeout' => 10.0,          // Total request timeout in seconds
            'connect_timeout' => 5.0,   // Connection timeout in seconds
            'headers' => [
                'Accept' => 'application/json',
                'User-Agent' => 'MyApp/1.0 (PHP Guzzle)',
            ],
            'handler' => $stack,
            'http_errors' => true,      // Throw exceptions for 4xx/5xx responses
        ]);
    }
}

// Usage in a service container or dependency injection
// $client = MyApiClientFactory::create($myLogger);
// $response = $client->get('users');
```

### ❌ 坏处：全局单例和超时缺失

```php
<?php

use GuzzleHttp\Client;

// Client created ad-hoc, no shared configuration, no timeouts
// Hard to test, prone to hanging requests
class LegacyService
{
    public function fetchUsers(): array
    {
        $client = new Client(); // ❌ BAD: No timeouts, no base_uri, no shared config
        $response = $client->get('https://api.example.com/v1/users');
        return json_decode($response->getBody()->getContents(), true);
    }
}
```

## 2. Middleware 用于跨领域关注点

利用“HandlerStack”和“GuzzleHttp\Middleware”实现可重用逻辑，例如重试、日志记录和 authentication。这可以让您的请求代码保持干燥。

### ✅ 好：集中 Middleware 用于身份验证和重试

（有关 middleware 的全面用法，请参阅上面的 `MyApiClientFactory::create` 示例。）

```php
<?php

use GuzzleHttp\Middleware;
use Psr\Http\Message\RequestInterface;

// Example of adding an authentication header via middleware
$stack->push(Middleware::mapRequest(function (RequestInterface $request) {
    $token = 'your_api_token_here'; // Get from secure config
    return $request->withHeader('Authorization', 'Bearer ' . $token);
}));
```

### ❌ 不好：每个请求重复逻辑

```php
<?php

use GuzzleHttp\Client;

class UserService
{
    private Client $client;

    public function __construct(Client $client)
    {
        $this->client = $client;
    }

    public function getUser(string $id): array
    {
        // ❌ BAD: Authentication header added manually for every request type
        $response = $this->client->get("users/{$id}", [
            'headers' => [
                'Authorization' => 'Bearer ' . 'your_api_token_here',
            ],
        ]);
        return json_decode($response->getBody()->getContents(), true);
    }
}
```

## 3. 库/SDK 的“自带客户端”(BYOC)

构建可重用库或 SDK 时，**永远不要硬依赖 Guzzle**。相反，键入提示 `Psr\Http\Client\ClientInterface` (PSR-18) 和 `Psr\Http\Message\RequestFactoryInterface` (PSR-17)。让消费者注入他们首选的 HTTP 客户端。

### ✅ 良好：符合 PSR-18/17 SDK

```php
<?php

use Psr\Http\Client\ClientInterface;
use Psr\Http\Message\RequestFactoryInterface;
use Psr\Http\Message\StreamFactoryInterface;
use Psr\Http\Message\ResponseInterface;

final class MySdkClient
{
    private const string BASE_URI = 'https://api.myservice.com';

    public function __construct(
        private ClientInterface $httpClient,
        private RequestFactoryInterface $requestFactory,
        private StreamFactoryInterface $streamFactory,
    ) {}

    public function fetchData(string $endpoint, array $data = []): ResponseInterface
    {
        $body = $this->streamFactory->createStream(json_encode($data));
        $request = $this->requestFactory->createRequest('POST', self::BASE_URI . $endpoint)
            ->withHeader('Content-Type', 'application/json')
            ->withBody($body);

        return $this->httpClient->sendRequest($request);
    }
}

// Consumer code (e.g., in a Symfony/Laravel app)
// use GuzzleHttp\Psr7\HttpFactory;
// use GuzzleHttp\Client as GuzzleClient;
// $guzzleClient = new GuzzleClient(/* ... config ... */);
// $httpFactory = new HttpFactory(); // PSR-17 implementation
// $sdk = new MySdkClient($guzzleClient, $httpFactory, $httpFactory);
```

### ❌ 不好：对 SDK 中的 Guzzle 的硬依赖

```php
<?php

use GuzzleHttp\Client; // ❌ BAD: Direct Guzzle dependency

final class MyBadSdkClient
{
    private Client $httpClient;

    public function __construct(Client $httpClient) // ❌ BAD: Forces Guzzle on consumers
    {
        $this->httpClient = $httpClient;
    }

    public function fetchData(string $endpoint): array
    {
        $response = $this->httpClient->get('https://api.myservice.com' . $endpoint);
        return json_decode($response->getBody()->getContents(), true);
    }
}
```

## 4. 错误处理

始终捕获特定的 Guzzle 异常，尤其是“RequestException”（针对 HTTP 错误）和“ConnectException”（针对网络问题）。

### ✅ 好：具体的异常处理

```php
<?php

use GuzzleHttp\Client;
use GuzzleHttp\Exception\RequestException;
use GuzzleHttp\Exception\ConnectException;

final class DataFetcher
{
    private Client $client;

    public function __construct(Client $client)
    {
        $this->client = $client;
    }

    public function fetchImportantData(): ?array
    {
        try {
            $response = $this->client->get('critical-endpoint');
            return json_decode($response->getBody()->getContents(), true);
        } catch (ConnectException $e) {
            // Log network error, notify ops, potentially retry (if not handled by middleware)
            error_log("Network error fetching data: " . $e->getMessage());
            return null; // Or throw a custom application exception
        } catch (RequestException $e) {
            // Log HTTP error (4xx, 5xx), inspect response
            error_log("HTTP error fetching data: " . $e->getMessage() . " Response: " . $e->getResponse()?->getBody());
            if ($e->hasResponse() && $e->getResponse()->getStatusCode() === 404) {
                return []; // Handle specific 404 case
            }
            return null; // Or throw a custom application exception
        } catch (\Throwable $e) {
            // Catch any other unexpected errors
            error_log("Unexpected error: " . $e->getMessage());
            throw $e; // Re-throw for higher-level handling
        }
    }
}
```

### ❌ 不好：捕获通用异常

```php
<?php

// ... (class definition)

    public function fetchImportantData(): ?array
    {
        try {
            $response = $this->client->get('critical-endpoint');
            return json_decode($response->getBody()->getContents(), true);
        } catch (\Exception $e) { // ❌ BAD: Catches too broadly, hides specific issues
            error_log("Error fetching data: " . $e->getMessage());
            return null;
        }
    }
```

## 5. 测试 Guzzle 客户端

使用“GuzzleHttp\Handler\MockHandler”进行确定性和快速的单元测试。这可以避免影响真实的 API 并确保一致的 test 结果。

### ✅ 好：使用 `MockHandler` 进行模拟响应

```php
<?php

use GuzzleHttp\Client;
use GuzzleHttp\Handler\MockHandler;
use GuzzleHttp\HandlerStack;
use GuzzleHttp\Psr7\Response;
use PHPUnit\Framework\TestCase;

final class MyServiceTest extends TestCase
{
    public function testFetchUsersSuccessfully(): void
    {
        // Create a mock handler with a successful response
        $mock = new MockHandler([
            new Response(200, ['Content-Type' => 'application/json'], json_encode(['users' => ['Alice', 'Bob']])),
        ]);

        $handlerStack = HandlerStack::create($mock);
        $client = new Client(['handler' => $handlerStack]);

        // Inject the mocked client into your service
        $service = new MyService($client);
        $users = $service->fetchUsers();

        $this->assertEquals(['Alice', 'Bob'], $users);
    }

    public function testFetchUsersNotFound(): void
    {
        // Mock a 404 response
        $mock = new MockHandler([
            new Response(404, [], 'Not Found'),
        ]);

        $handlerStack = HandlerStack::create($mock);
        $client = new Client(['handler' => $handlerStack]);

        $service = new MyService($client);

        // Expect an exception or specific return based on your error handling
        $this->expectException(\GuzzleHttp\Exception\ClientException::class); // If http_errors is true
        $service->fetchUsers();
    }
}

// Example MyService class
class MyService
{
    private Client $client;

    public function __construct(Client $client)
    {
        $this->client = $client;
    }

    public function fetchUsers(): array
    {
        $response = $this->client->get('users');
        return json_decode($response->getBody()->getContents(), true)['users'];
    }
}
```

### ❌ 不好：在测试中使用真实的 API

```php
<?php

use GuzzleHttp\Client;
use PHPUnit\Framework\TestCase;

final class MyServiceIntegrationTest extends TestCase
{
    public function testFetchUsersFromRealApi(): void
    {
        // ❌ BAD: Hits a real external API, making tests slow, flaky, and dependent on external services
        $client = new Client(['base_uri' => 'https://api.example.com/v1/']);
        $service = new MyService($client);
        $users = $service->fetchUsers();

        $this->assertIsArray($users);
        $this->assertNotEmpty($users);
    }
}
```

## 6. Laravel HTTP 客户端

如果您在 Laravel 项目中，请使用 Laravel 的内置 HTTP 客户端包装器。它由 Guzzle 提供支持，但提供更流畅的 API、内置伪造和并发请求帮助程序。

### ✅ 好：Laravel HTTP 客户端

```php
<?php

use Illuminate\Support\Facades\Http;

final class LaravelIntegrationService
{
    public function getUserData(int $userId): array
    {
        $response = Http::withToken('your_api_token')
                        ->timeout(10)
                        ->retry(3, 100) // Retry 3 times, 100ms delay
                        ->get("https://api.example.com/users/{$userId}");

        $response->throw(); // Throws an exception for 4xx/5xx responses

        return $response->json();
    }

    public function sendConcurrentRequests(): array
    {
        $responses = Http::pool(fn (Http\Client\Pool $pool) => [
            $pool->get('https://api.example.com/data/1'),
            $pool->get('https://api.example.com/data/2'),
        ]);

        return [
            $responses[0]->json(),
            $responses[1]->json(),
        ];
    }
}
```