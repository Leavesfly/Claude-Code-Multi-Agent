---
description: Definitive guidelines for writing robust, maintainable, and backend-agnostic asynchronous Python code using AnyIO. Prioritize structured concurrency and native AnyIO primitives.
---

# AnyIO 最佳实践

AnyIO 是现代 Python 异步开发的权威选择。它在 `asyncio` 或 `Trio` 之上提供了一套受 Trio 启发的统一 API，修复了 `asyncio` 关键的设计缺陷并强制结构化并发。遵循这些规则构建可靠、高性能的异步应用。

## 1. 入口点与 backend 选择

始终使用 `anyio.run()` 作为应用入口。明确选择你偏好的 backend，并利用其特定优化。

❌ **差**：依赖默认 backend 或 `asyncio.run()`。
```python
import asyncio
from anyio import run, sleep

async def main():
    print("Running on default backend (asyncio) without uvloop.")
    await sleep(0.1)

asyncio.run(main) # Bypasses AnyIO's entry point
run(main) # Implicit backend, no optimizations
```

✅ **好**：明确选择 backend 并启用性能优化。
```python
from anyio import run, sleep
import sniffio

async def main():
    backend = sniffio.current_async_library()
    print(f"Hello from {backend}!")
    await sleep(0.1)

# For production, prefer