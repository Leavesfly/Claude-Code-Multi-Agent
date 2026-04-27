---
description: This guide provides opinionated, actionable best practices for writing robust, performant, and maintainable Puppeteer scripts for web scraping, automation, and testing. It emphasizes modern patterns like `page.locator()` and efficient resource management.
---

# puppeteer 最佳实践

Puppeteer 是 Node.js 中无头浏览器自动化的权威库。遵循这些准则来构建可靠、快速且可维护的脚本。

## 1. 基础：Async/Await 和浏览器管理

始终使用“async/await”来获得清晰、顺序的异步代码。仔细管理浏览器实例。

```javascript
// ❌ BAD: Synchronous or callback-hell
// puppeteer.launch().then(browser => { ... });

// ✅ GOOD: Clear async/await structure
import puppeteer from 'puppeteer';

async function runAutomation() {
  const browser = await puppeteer.launch({ headless: 'new' }); // Always use 'new' headless mode
  const page = await browser.newPage();

  try {
    // ... automation logic ...
  } catch (error) {
    console.error('Automation failed:', error);
  } finally {
    await browser.close(); // Ensure browser always closes
  }
}

runAutomation();
```

## 2. 交互：拥抱“page.locator()”

“page.locator()”是与元素交互的现代、强大的方式。它自动处理等待元素可见、启用和稳定的情况，从而消除竞争条件。

```javascript
// ❌ BAD: Manual waiting and brittle selectors
// await page.waitForSelector('.my-button');
// await page.click('.my-button');
// await page.$eval('h1', el => el.innerText); // No auto-waiting for element presence

// ✅ GOOD: Use page.locator() for all interactions
async function interactWithPage(page) {
  // Click a button
  await page.locator('button.submit-btn').click();

  // Fill an input field
  await page.locator('input[name="username"]').fill('myUsername');

  // Extract text with auto-waiting
  const title = await page.locator('h1.page-title').evaluate(el => el.innerText);
  console.log('Page Title:', title);

  // Wait for an element to appear without interacting
  await page.locator('.loading-spinner').wait({ state: 'hidden' }); // Wait for it to disappear
}
```

## 3. 效率和性能

通过减少不必要的资源加载和利用并行执行来优化 Puppeteer 脚本。

### 3.1.禁用不必要的资源

块图像、CSS 和字体可显着加快页面加载速度，尤其是抓取速度。

```javascript
// ❌ BAD: Loading all resources, wasting bandwidth and time
// await page.goto('https://example.com');

// ✅ GOOD: Intercept and block non-essential resources
async function optimizePageLoad(page) {
  await page.setRequestInterception(true);
  page.on('request', (request) => {
    if (['image', 'stylesheet', 'font', 'media'].includes(request.resourceType())) {
      request.abort();
    } else {
      request.continue();
    }
  });
  await page.goto('https://example.com', { waitUntil: 'domcontentloaded' }); // Use 'domcontentloaded' when possible
}
```

### 3.2.使用 Promise.all() 并行化操作

当处理多个页面或独立任务时，并发运行它们。

```javascript
// ❌ BAD: Sequential processing of multiple URLs
// for (const url of urls) { await processUrl(page, url); }

// ✅ GOOD: Process multiple URLs in parallel
async function processMultipleUrls(browser, urls) {
  const pagePromises = urls.map(async (url) => {
    const page = await browser.newPage();
    try {
      await page.goto(url, { waitUntil: 'domcontentloaded' });
      const data = await page.locator('h1').evaluate(el => el.innerText);
      return { url, data };
    } catch (error) {
      console.error(`Failed to process ${url}:`, error);
      return { url, error: error.message };
    } finally {
      await page.close(); // Close page after processing
    }
  });
  return Promise.all(pagePromises);
}
```

## 4. 稳健性和反机器人措施

构建弹性抓取器，可以优雅地处理故障并避免检测。

### 4.1.使用指数退避实现重试逻辑

网络问题和暂时性错误很常见。重试失败的操作并增加延迟。

```javascript
// ❌ BAD: No retry logic, script fails on first error
// await page.click('.flaky-button');

// ✅ GOOD: Robust retry mechanism
async function clickWithRetry(page, selector, retries = 3, delay = 1000) {
  for (let i = 0; i < retries; i++) {
    try {
      await page.locator(selector).click({ timeout: 5000 });
      return; // Success
    } catch (error) {
      console.warn(`Attempt ${i + 1} failed for ${selector}: ${error.message}`);
      if (i < retries - 1) {
        await page.waitForTimeout(delay * Math.pow(2, i)); // Exponential back-off
      } else {
        throw new Error(`Failed to click ${selector} after ${retries} attempts.`);
      }
    }
  }
}
// Usage: await clickWithRetry(page, 'button.flaky-submit');
```

### 4.2.集成隐形插件以实现反机器人规避

网站主动检测自动浏览器。使用“puppeteer-extra”和“puppeteer-extra-plugin-stealth”。

```javascript
// ❌ BAD: Easily detectable as a bot
// const browser = await puppeteer.launch();

// ✅ GOOD: Launch with stealth plugin
import puppeteer from 'puppeteer-extra';
import StealthPlugin from 'puppeteer-extra-plugin-stealth';

puppeteer.use(StealthPlugin());

async function launchStealthBrowser() {
  const browser = await puppeteer.launch({
    headless: 'new',
    args: [
      '--no-sandbox', // Essential for CI/Docker
      '--disable-setuid-sandbox',
      '--disable-dev-shm-usage', // Essential for CI/Docker
      '--disable-accelerated-2d-canvas',
      '--no-first-run',
      '--no-zygote',
      '--single-process', // For CI/Docker
      '--disable-gpu'
    ]
  });
  const page = await browser.newPage();
  await page.setUserAgent('Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36');
  await page.setViewport({ width: 1920, height: 1080 }); // Realistic viewport
  return { browser, page };
}
```

## 5. 代码质量：模块化和类型化

将您的代码组织成有重点的、可重用的函数。对于较大的项目，请考虑 TypeScript。

```javascript
// ❌ BAD: Monolithic script
// (async () => { /* hundreds of lines of mixed logic */ })();

// ✅ GOOD: Modular functions for clarity and reusability
// scraper.js
import puppeteer from 'puppeteer';

async function navigateAndLogin(page, username, password) {
  await page.goto('https://example.com/login');
  await page.locator('#username').fill(username);
  await page.locator('#password').fill(password);
  await page.locator('button[type="submit"]').click();
  await page.waitForNavigation({ waitUntil: 'networkidle0' });
}

async function extractProductData(page) {
  const products = await page.locator('.product-item').evaluateAll(items =>
    items.map(item => ({
      name: item.querySelector('.product-name')?.innerText,
      price: item.querySelector('.product-price')?.innerText,
    }))
  );
  return products;
}

export async function scrapeWebsite(url, credentials) {
  const browser = await puppeteer.launch({ headless: 'new' });
  const page = await browser.newPage();
  try {
    await navigateAndLogin(page, credentials.username, credentials.password);
    await page.goto(url);
    const data = await extractProductData(page);
    return data;
  } finally {
    await browser.close();
  }
}

// main.js
// import { scrapeWebsite } from './scraper.js';
// (async () => {
//   const data = await scrapeWebsite('https://example.com/products', { username: 'user', password: 'pass' });
//   console.log(data);
// })();
```