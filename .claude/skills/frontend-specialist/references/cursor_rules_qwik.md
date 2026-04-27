---
description: This guide outlines definitive best practices for building high-performance, resumable Qwik applications, focusing on maximizing lazy loading and ensuring optimal developer experience.
---

# qwik 最佳实践

Qwik 是一个基于**可恢复性**而不是水化构建的边缘优先框架。这意味着您的应用程序的 JavaScript 仅在绝对必要时才加载和执行，从而实现即时交互时间 (TTI)。我们的指导方针优先考虑这一核心原则。

## 关键准则：

### 1. 使用 `component$` 定义每个 UI 部分

所有交互式 UI 组件**必须**包装在 `component$` 中。这是 Qwik 启用自动代码分割和延迟加载的基本原语。

❌ **不好：常规函数 component**
```typescript
// my-button.tsx
export const MyButton = () => { // Missing '$'
  return <button onClick={() => alert('Clicked!')}>Click me</button>;
};
```

✅ **好：`component$` 可恢复性**
```typescript
// my-button.tsx
import { component$ } from '@builder.io/qwik';

export const MyButton = component$(() => {
  return <button onClick$={() => alert('Clicked!')}>Click me</button>; // onClick$ is also a QRL
});
```

### 2. 使用`useStore`来实现反应状态

使用“useStore”管理 component-本地反应状态。这确保了状态在服务器端渲染和客户端交互中是可序列化和可恢复的。为了清晰和安全起见，请务必输入您的商店。

❌ **坏：非反应性或不可序列化状态**
```typescript
// counter.tsx
import { component$ } from '@builder.io/qwik';

export const Counter = component$(() => {
  let count = 0; // Not reactive, won't persist across renders/resumability
  const increment = () => { count++; };
  return <button onClick$={increment}>{count}</button>;
});
```

✅ **好：`useStore` 具有 TypeScript 接口**
```typescript
// counter.tsx
import { component$, useStore } from '@builder.io/qwik';

interface CounterStore {
  count: number;
}

export const Counter = component$(() => {
  const store = useStore<CounterStore>({ count: 0 });
  return (
    <button onClick$={() => store.count++}>
      Count: {store.count}
    </button>
  );
});
```

### 3. 使用“useVisibleTask$”推迟客户端效果

仅当 component 在视口中可见时，才执行客户端特定的逻辑，例如第三方库初始化或 DOM 操作。这可以防止在初始加载时不必要的 JavaScript 执行。对于“必须”在客户端上运行（无论可见性如何）的效果，请使用“useClientEffect$”。

❌ **不好：component 渲染中急切的客户端效果**
```typescript
// chart-component.tsx
import { component$ } from '@builder.io/qwik';
import Chart from 'chart.js'; // This will be bundled and executed eagerly

export const ChartComponent = component$(() => {
  // This code runs on the server and client, potentially causing errors or unnecessary work
  const canvasRef = useRef<HTMLCanvasElement>();
  useEffect(() => { // React-style hook, not Qwik-native
    if (canvasRef.current) {
      new Chart(canvasRef.current, { /* ... */ });
    }
  }, []);
  return <canvas ref={canvasRef}></canvas>;
});
```

✅ **好：`useVisibleTask$` 用于客户端效果**
```typescript
// chart-component.tsx
import { component$, useSignal, useVisibleTask$ } from '@builder.io/qwik';
// Import client-side library dynamically or ensure it's tree-shaken
import type Chart from 'chart.js'; // Use type import to avoid bundling

export const ChartComponent = component$(() => {
  const canvasRef = useSignal<HTMLCanvasElement>();

  useVisibleTask$(({ track }) => {
    track(() => canvasRef.value); // Re-run if canvasRef changes
    if (canvasRef.value) {
      // Dynamically import Chart.js to ensure it's only loaded on the client when needed
      import('chart.js').then(({ default: Chart }) => {
        new Chart(canvasRef.value!, {
          type: 'bar',
          data: {
            labels: ['Red', 'Blue', 'Yellow'],
            datasets: [{
              label: '# of Votes',
              data: [12, 19, 3],
              backgroundColor: ['red', 'blue', 'yellow'],
            }],
          },
        });
      });
    }
  });

  return <canvas ref={canvasRef}></canvas>;
});
```

### 4. 对所有 QRL 使用“$”后缀

任何需要由 Qwik 的优化器进行代码分割和延迟加载的函数**必须**以“$”后缀结尾。这包括组件 (`component$`)、事件处理程序 (`onClick$`、`onInput$`) 和挂钩 (`useStore`、`useTask$`、`useVisibleTask$`、`useClientEffect$`、`useResource$`、`useSignal$`)。

❌ **不好：事件处理程序中缺少 `$` 后缀**
```typescript
// button.tsx
import { component$ } from '@builder.io/qwik';

export const MyButton = component$(() => {
  const handleClick = () => { // Missing '$'
    console.log('Button clicked!');
  };
  return <button onClick={handleClick}>Click me</button>; // Will not be lazy-loaded
});
```

✅ **好：正确的 QRL 后缀“$”**
```typescript
// button.tsx
import { component$ } from '@builder.io/qwik';

export const MyButton = component$(() => {
  const handleClick = component$(() => { // Even internal functions can be QRLs
    console.log('Button clicked!');
  });
  return <button onClick$={handleClick}>Click me</button>; // Correctly lazy-loaded
});
```

### 5. 利用 Qwik City 进行路由和数据加载

对于使用 Qwik City 的应用程序，利用其基于文件系统的路由、布局和“loader$”函数来高效获取数据。这为服务器端数据加载和路由提供了零开销解决方案。

❌ **不好：手动路由或客户端数据获取以进行初始加载**
```typescript
// routes/products/index.tsx
import { component$, useClientEffect$, useStore } from '@builder.io/qwik';

export default component$(() => {
  const store = useStore({ products: [] });
  useClientEffect$(async () => { // Data fetched on client, after hydration
    const res = await fetch('/api/products');
    store.products = await res.json();
  });
  return <div>{/* ... render products ... */}</div>;
});
```

✅ **好：`loader$` 用于服务器端数据获取**
```typescript
// routes/products/index.tsx
import { component$ } from '@builder.io/qwik';
import { routeLoader$ } from '@builder.io/qwik-city';

interface Product {
  id: string;
  name: string;
}

// Data fetched on the server during SSR, then serialized and resumed on client
export const useProducts = routeLoader$(async () => {
  const res = await fetch('https://api.example.com/products'); // Fetch from an API
  const products: Product[] = await res.json();
  return products;
});

export default component$(() => {
  const products = useProducts(); // Access the loaded data
  return (
    <div>
      <h1>Products</h1>
      <ul>
        {products.value.map((product) => (
          <li key={product.id}>{product.name}</li>
        ))}
      </ul>
    </div>
  );
});
```

### 6. 使用 `PropFunction` 将函数作为 Prop 传递

当将函数作为 prop 传递给本身是 `component$` 的子 component 时，请确保 prop 类型为 `PropFunction<(...args: any[]) => any>`。这向 Qwik 发出信号，表明该函数是 QRL，应该进行序列化和延迟加载。

❌ **不好：将常规函数类型作为 prop 传递**
```typescript
// child-button.tsx
import { component$ } from '@builder.io/qwik';

interface ChildButtonProps {
  onClick: () => void; // Not a PropFunction
}

export const ChildButton = component$((props: ChildButtonProps) => {
  return <button onClick$={props.onClick}>Child Button</button>;
});

// parent-component.tsx
import { component$ } from '@builder.io/qwik';
import { ChildButton } from './child-button';

export const ParentComponent = component$(() => {
  const handleParentClick = component$(() => console.log('Parent handled click'));
  return <ChildButton onClick={handleParentClick} />; // Type mismatch, potential issues
});
```

✅ **好：使用 `PropFunction` 作为函数 props**
```typescript
// child-button.tsx
import { component$, type PropFunction } from '@builder.io/qwik';

interface ChildButtonProps {
  onClick$: PropFunction<() => void>; // Correctly typed as a PropFunction
}

export const ChildButton = component$((props: ChildButtonProps) => {
  return <button onClick$={props.onClick$}>Child Button</button>;
});

// parent-component.tsx
import { component$ } from '@builder.io/qwik';
import { ChildButton } from './child-button';

export const ParentComponent = component$(() => {
  const handleParentClick = component$(() => console.log('Parent handled click'));
  return <ChildButton onClick$={handleParentClick} />; // Correctly passing QRL
});
```

### 7. 采用原子 CSS （例如 Tailwind CSS）

集成诸如 Tailwind CSS 之类的样式解决方案，在构建时生成静态的、实用程序优先的类。这避免了运行时风格的计算，并使 CSS 有效负载保持最小，与 Qwik 的性能目标保持一致。

❌ **不好：运行时 CSS-in-JS 或复杂的动态样式**
```typescript
// styled-button.tsx
import { component$ } from '@builder.io/qwik';
import { css } from '@emotion/css'; // Runtime CSS-in-JS

const buttonStyle = css`
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  &:hover {
    background-color: darkblue;
  }
`;

export const StyledButton = component$(() => {
  return <button class={buttonStyle}>Styled Button</button>;
});
```

✅ **好：Tailwind CSS 用于静态样式**
```typescript
// tailwind-button.tsx
import { component$ } from '@builder.io/qwik';

export const TailwindButton = component$(() => {
  return (
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
      Tailwind Button
    </button>
  );
});
```

### 8.使用Vitest和Playwright进行测试

利用 Qwik 的内置测试堆栈：用于单元和 component 测试的 Vitest，以及用于端到端场景的 Playwright。这些工具针对 Qwik 的独特架构进行了集成和优化，确保您的延迟加载和可恢复性正常工作。

❌ **不好：使用 Jest/Enzyme 进行 Qwik component 测试**
```typescript
// my-component.test.js (using Jest)
import { render } from '@testing-library/react'; // Incorrect library for Qwik
import { MyComponent } from './my-component';

test('MyComponent renders correctly', () => {
  const { getByText } = render(<MyComponent />);
  expect(getByText('Hello')).toBeInTheDocument();
});
```

✅ **好：Vitest 进行 Qwik component 测试**
```typescript
// my-component.test.tsx (using Vitest)
import { render, screen } from '@builder.io/qwik/testing';
import { component$ } from '@builder.io/qwik';
import { MyComponent } from './my-component'; // Assuming MyComponent is a Qwik component

describe('MyComponent', () => {
  it('should render correctly', async () => {
    await render(<MyComponent />);
    expect(screen.getByText('Hello Qwik!')).toBeInTheDocument();
  });

  it('should handle click event', async () => {
    const mockFn = vi.fn();
    const TestComponent = component$(() => {
      return <button onClick$={mockFn}>Click Me</button>;
    });

    await render(<TestComponent />);
    await screen.getByRole('button', { name: 'Click Me' }).click();
    expect(mockFn).toHaveBeenCalledTimes(1);
  });
});
```