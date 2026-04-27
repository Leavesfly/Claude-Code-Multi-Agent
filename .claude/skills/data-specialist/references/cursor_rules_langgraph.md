---
description: This guide provides definitive best practices for building robust, maintainable, and performant AI agent workflows using LangGraph.
---

# langgraph 最佳实践

## 代码组织与结构

### 1. 显式的图拓扑与模块化
为每个 node 定义清晰且单一的职责。将通用模式封装为可复用的 subgraph。优先使用有向无环图（DAG）；仅在反馈回路确有必要时才使用环。

✅ 好：职责单一的 node，可组合的 subgraph
```python
from langgraph.graph import StateGraph, END
from typing import TypedDict, List

class AgentState(TypedDict):
    messages: List[str]
    tool_output: str | None

def fetch_data_node(state: AgentState) -> AgentState:
    return {"messages": state["messages"] + ["Data fetched."]}

def analyze_data_node(state: AgentState) -> AgentState:
    return {"messages": state["messages"] + ["Data analyzed."]}

def build_subgraph():
    builder = StateGraph(AgentState)
    builder.add_node("fetch", fetch_data_node)
    builder.add_node("analyze", analyze_data_node)
    builder.add_edge("fetch", "analyze")
    builder.set_entry_point("fetch")
    builder.set_finish_point("analyze")
    return builder.compile()
```

### 2. 优先使用函数式 API
Prior