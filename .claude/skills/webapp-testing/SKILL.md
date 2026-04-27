---
name: webapp-testing
description: 使用 Playwright 与本地 Web 应用程序交互和测试的工具包。支持验证前端功能、调试 UI 行为、捕获浏览器截图和查看浏览器日志。
license: Complete terms in LICENSE.txt
---

# Web Application Testing（Web 应用测试）

用于测试本地 Web 应用，直接编写原生 Python Playwright 脚本。

**可用的 helper 脚本**：
- `scripts/with_server.py` — 管理 server 生命周期（支持多个 server）

**始终先用 `--help` 运行脚本** 查看用法。在确认必须做定制化之前，**不要**先去读脚本源码。这些脚本可能很长，会污染你的 context window。它们被设计成黑盒直接调用，而不是读进上下文里。

## 决策树：选择合适的方式

```
User task → Is it static HTML?
    ├─ Yes → Read HTML file directly to identify selectors
    │         ├─ Success → Write Playwright script using selectors
    │         └─ Fails/Incomplete → Treat as dynamic (below)
    │
    └─ No (dynamic webapp) → Is the server already running?
        ├─ No → Run: python scripts/with_server.py --help
        │        Then use the helper + write simplified Playwright script
        │
        └─ Yes → Reconnaissance-then-action:
            1. Navigate and wait for networkidle
            2. Take screenshot or inspect DOM
            3. Identify selectors from rendered state
            4. Execute actions with discovered selectors
```

## 示例：使用 with_server.py

要启动 server，先跑 `--help`，再用 helper：

**单 server：**
```bash
python scripts/with_server.py --server "npm run dev" --port 5173 -- python your_automation.py
```

**多 server（如 backend + frontend）：**
```bash
python scripts/with_server.py \
  --server "cd backend && python server.py" --port 3000 \
  --server "cd frontend && npm run dev" --port 5173 \
  -- python your_automation.py
```

编写自动化脚本时，只包含 Playwright 逻辑即可（server 生命周期由 helper 自动管理）：
```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True) # Always launch chromium in headless mode
    page = browser.new_page()
    page.goto('http://localhost:5173') # Server already running and ready
    page.wait_for_load_state('networkidle') # CRITICAL: Wait for JS to execute
    # ... your automation logic
    browser.close()
```

## 先侦察、再行动（Reconnaissance-Then-Action）模式

1. **检查已渲染的 DOM**：
   ```python
   page.screenshot(path='/tmp/inspect.png', full_page=True)
   content = page.content()
   page.locator('button').all()
   ```

2. **识别 selector** — 从检查结果里找

3. **执行操作** — 用找到的 selector 执行动作

## 常见坑

❌ **不要**在动态应用里没等到 `networkidle` 就去检查 DOM
✅ **要**先 `page.wait_for_load_state('networkidle')` 再做检查

## 最佳实践

- **把打包脚本当黑盒用** — 要完成任务前，先看看 `scripts/` 里有没有现成的脚本能帮上忙。这些脚本已经可靠地处理了常见的复杂 workflow，而且不会污染你的 context window。用 `--help` 看用法，然后直接调用。
- 同步脚本用 `sync_playwright()`
- 结束时一定要关闭 browser
- 使用有描述性的 selector：`text=`、`role=`、CSS selector 或 ID
- 加合适的等待：`page.wait_for_selector()` 或 `page.wait_for_timeout()`

## 参考文件

- **examples/** — 展示常见模式的示例：
  - `element_discovery.py` — 发现页面上的按钮、链接和输入框
  - `static_html_automation.py` — 对本地 HTML 使用 file:// URL
  - `console_logging.py` — 在自动化过程中捕获 console log