---
description: Provides definitive best practices for using Tailwind CSS, focusing on maintainability, performance, and scalability in modern web development.
---

# tailwind 最佳实践

Tailwind CSS 是一个强大的实用程序优先框架。这些规则确保我们的项目有效地利用 Tailwind，促进整个团队的一致性、性能和可维护性。

## 1. 设计系统配置

**始终在“tailwind.config.js”中定义您的设计系统。**避免标记中的任意值，除非对于独特的、不可重用的样式绝对必要。这可确保 UI 一致性并实现高效的 PurgeCSS 树摇动。

❌ 不好：标记中的任意值
```html
<div class="bg-[#1a2b3c] text-[15px] p-[1.25rem] rounded-[8px]">...</div>
```

✅ 好：配置的设计令牌
```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: { 500: '#1a2b3c' },
      },
      fontSize: {
        'body-md': '15px',
      },
      spacing: {
        'layout-sm': '1.25rem',
      },
      borderRadius: {
        'card': '8px',
      },
    },
  },
};
```
```html
<div class="bg-brand-500 text-body-md p-layout-sm rounded-card">...</div>
```

## 2. Component 抽象

**将可重用的 UI 模式提取到特定于框架的组件中。** 这可以使您的标记保持干净，促进可重用性并集中复杂的样式逻辑。使用“clsx”或“cva”等库进行动态类组合。

**避免使用 `@apply` 进行一般 component 样式。** 它创建自定义 CSS ，隐藏 Tailwind 实用程序优先的性质，使重构更加困难并降低清晰度。为非常具体的、不可重用的基本样式（例如，重置浏览器默认值）或遗留迁移保留“@apply”。

❌ 不好：每个实例中都有重复的实用程序汤
```html
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-hidden focus:ring-2 focus:ring-blue-500/50">
  Submit
</button>
<!-- ... many more buttons with the exact same classes ... -->
```

✅ 好：可重复使用 component （React 示例）
```jsx
// components/Button.jsx
import clsx from 'clsx';

export function Button({ variant = 'primary', size = 'md', className, ...props }) {
  const baseStyles = 'font-bold py-2 px-4 rounded focus:outline-hidden focus:ring-2';
  const variantStyles = {
    primary: 'bg-blue-500 hover:bg-blue-700 text-white focus:ring-blue-500/50',
    secondary: 'bg-gray-200 hover:bg-gray-300 text-gray-800 focus:ring-gray-300/50',
  };
  const sizeStyles = {
    sm: 'text-sm',
    md: 'text-base',
  };

  return (
    <button
      className={clsx(baseStyles, variantStyles[variant], sizeStyles[size], className)}
      {...props}
    />
  );
}
```
```jsx
// Usage
<Button>Submit</Button>
<Button variant="secondary">Cancel</Button>
```

## 3. 类的顺序和可读性

**对实用程序类进行逻辑分组以提高可读性。** 虽然 Tailwind 不强制执行顺序，但一致的模式使扫描类变得更容易并减少认知负担。我们推荐：
`布局` > `Flexbox` > `网格` > `盒子模型（p、m、w、h）` > `版式` > `背景` > `边框` > `效果` > `交互性` > `状态（悬停、焦点、活动、响应）`

❌ 不好：随机顺序
```html
<div class="text-lg flex shadow-md p-4 items-center bg-white rounded-lg">...</div>
```

✅ 好：顺序一致
```html
<div class="flex items-center p-4 bg-white rounded-lg shadow-md text-lg">...</div>
```

## 4. 移动优先和响应式设计

**采用移动优先的方法。** 首先针对最小屏幕进行设计，然后使用响应式前缀（`sm:`、`md:`、`lg:`、`xl:`、`2xl:`）来适应更大视口的样式。这是使用 Tailwind 构建响应式 UI 的默认且最有效的方法。

```html
<!-- Default (mobile) is stacked, medium screens and up are side-by-side -->
<div class="flex flex-col md:flex-row items-center md:items-start gap-4">
  <img class="w-24 h-24 rounded-full md:w-32 md:h-32" src="..." alt="Profile">
  <div class="text-center md:text-left">
    <h2 class="text-xl md:text-2xl font-bold">John Doe</h2>
    <p class="text-gray-600 md:text-lg">Software Engineer</p>
  </div>
</div>
```

## 5. 性能优化

**为 PurgeCSS 配置 `content` 并利用 JIT。** 确保您的 `tailwind.config.js` 准确列出包含 Tailwind 类的*所有*文件。这对于 PurgeCSS 来说至关重要，它可以对未使用的 CSS 进行 tree-shake，从而使包大小保持最小。 Tailwind v4 默认使用 Just-In-Time (JIT) 引擎，提供快速编译。

```js
// tailwind.config.js
module.exports = {
  content: [
    './src/**/*.{js,jsx,ts,tsx,vue,html}', // Your source files
    './public/index.html', // Any raw HTML files
  ],
  theme: { extend: {} },
  plugins: [],
};
```

**保持 Tailwind 更新。** 使用 `npx @tailwindcss/upgrade` 从 v3 迁移到 v4，受益于性能改进和现代 CSS 功能。请注意 v4 的现代浏览器要求（Safari 16.4+、Chrome 111+、Firefox 128+）。

## 6. 辅助功能

**优先考虑语义 HTML。** Tailwind 样式应该增强而不是取代正确的 HTML 结构。确保所有交互元素都有清晰可见的焦点状态。

**`outline-hidden` 与 `outline-none` (Tailwind v4)：**
* **`轮廓隐藏`**：设置一个不可见的轮廓，*仍然以强制颜色模式显示*以方便访问。 **当您想要隐藏默认浏览器轮廓但确保可访问性时，首选交互元素**。
* **`outline-none`**：设置`outline-style: none`，完全去除轮廓。 **对于交互式元素，请避免这种情况**，除非您提供自定义的、可访问的焦点指示器（例如“focus:ring-2”）。

❌ 坏处：完全失去焦点
```html
<button class="focus:outline-none">Click me</button>
```

✅ 好：可访问的焦点状态
```html
<button class="focus:outline-hidden focus:ring-2 focus:ring-blue-500/50">Click me</button>
```

## 7. 主题和深色模式

**为自定义主题扩展“tailwind.config.js”。**定义自定义颜色、字体、间距和其他设计标记以匹配您的品牌。利用“dark:”变体获得内置暗模式支持。

```js
// tailwind.config.js
module.exports = {
  darkMode: 'class', // or 'media'
  theme: {
    extend: {
      colors: {
        'primary-light': '#f0f9ff',
        'primary-dark': '#0c4a6e',
      },
    },
  },
};
```
```html
<div class="bg-primary-light dark:bg-primary-dark text-gray-900 dark:text-gray-100">
  Hello, world!
</div>
```