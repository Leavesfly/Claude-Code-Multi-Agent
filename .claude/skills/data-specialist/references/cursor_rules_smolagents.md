---
description: Definitive guidelines for building robust, performant, and maintainable AI agents using smolagents, focusing on practical patterns and avoiding common pitfalls.
---

# smolagents 最佳实践

本指南给出开发 `smolagents` 应用的权威最佳实践。遵循这些原则能让你的 agent 可靠、高效、易于维护。

## 1. 代码组织与结构

把 agent 逻辑和 tool 组织到清晰独立的模块中。这能提升可读性、可复用性和可测试性。

❌ 差：单体 agent 文件
```python
# agent.py
from smolagents import CodeAgent, HfApiModel, tool
import requests # Direct import in agent file

@tool
def search_web(query: str) -> str:
    # ... complex search logic ...
    return "search result"

@tool
def calculate_math(expression: str) -> float:
    # ... math logic ...
    return eval(expression) # DANGER! Unsafe!

agent = CodeAgent(tools=[search_web, calculate_math], model=HfApiModel())
agent.run("What is 2+2 and also search for 'latest AI news'")
```

✅ 好：模块化结构
```python
# src/tools.py
from smolagents import tool
import requests
import json

@tool
def web_search(query: str) -> str:
    """
    Performs a web search for the given query.
    Args:
        query: The search term.
    Returns:
        A summary of the search results or an error message.
    """
    try:
        # Replace with a real search API (e.g., DuckDuckGoSearchTool, custom API)
        response = requests.get(f"https://api.example.com/search?q