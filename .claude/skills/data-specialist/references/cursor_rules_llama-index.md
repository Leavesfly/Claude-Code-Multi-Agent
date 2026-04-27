---
description: Definitive guidelines for building production-ready LlamaIndex applications, emphasizing modularity, type safety, cloud services, and robust testing.
---

# LlamaIndex 最佳实践

LlamaIndex 是构建上下文增强型 LLM 应用程序的权威框架。遵循这些规则以确保你的 RAG 管道和 agent 在 2025 年是模块化、类型安全、高性能且生产就绪的。

## 1. 代码组织和结构

将 LlamaIndex 组件组织成不同的、可测试的函数或类。利用 `Workflow` API 实现显式数据流。

### ✅ 好:模块化函数和 Workflow API

```python
# core_components.py
from llama_index.core import Document, VectorStoreIndex, Settings
from llama_index.core.readers import SimpleDirectoryReader
from llama_index.llms.openai import OpenAI
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.core.workflow import Workflow, LlamaAgentWorker, AgentInput, AgentOutput
from typing import List

def configure_global_settings() -> None:
    """Configures global LLM and embedding models for consistent behavior."""
    Settings.llm = OpenAI(model="gpt-4o-mini", temperature=0.1)
    Settings.embed_model = OpenAIEmbedding(model="text-embedding-3-small")

def load_documents(data_path: str) -> List[Document]:
    """Loads documents from a directory using SimpleDirectoryReader."""
    return SimpleDirectoryReader(data_path).load_data()

def build_vector_index(documents: List[Document]) -> VectorStoreIndex:
    """Builds a VectorStoreIndex from documents."""
    return VectorStoreIndex.from_documents(documents)

def create_query_engine(index: VectorStoreIndex):
    """Creates a query engine with optimal response mode for citations."""
    return index.as_query_engine(response_mode="compact", verbose=True)

# main_app.py
from core_components import (
    configure_global_settings, load_documents, build_vector_index, create_query_engine
)

def main_rag_workflow():
    configure_global_settings() # Always configure settings first

    docs = load_documents("./data")
    index = build_vector_index(docs)
    query_engine = create_query_engine(index)

    # Wrap the query engine in an AgentWorker for Workflow compatibility
    agent_worker = LlamaAgentWorker.from_query_engine(query_engine)
    rag_workflow = Workflow(
        name="SimpleRAGWorkflow",
        description="A basic RAG pipeline for document querying.",
        input_type=AgentInput,
        output_type=AgentOutput,
        worker=agent_worker
    )

    result = rag_workflow.run(AgentInput(query="What are the main themes across these documents?"))
    print(result.response)

if __name__ == "__main__":
    main_rag_workflow()
```

### ❌ 差:单体脚本

```python
# app.py (avoid this structure for anything beyond quickstarts)
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, Settings
from llama_index.llms.openai import OpenAI
from llama_index.embeddings.openai import OpenAIEmbedding

Settings.llm = OpenAI(model="gpt-4o-mini")
Settings.embed_model = OpenAIEmbedding(model="text-embedding-3-small")

docs = SimpleDirectoryReader("data").load_data()
index = VectorStoreIndex.from_documents(docs)
qe = index.as_query_engine()
ans = qe.query("What are the main themes?")
print(ans)
```

## 2. 常见模式和反模式

### ✅ 好:利用 LlamaCloud 服务和 LlamaHub 连接器

对于生产级 RAG,优先使用托管服务如 LlamaParse 和 LlamaCloud,并利用 LlamaHub 获取数据连接器。

```python
import os
from llama_index.core.readers import LlamaParseReader
from llama_index.cloud import LlamaCloudIndex
from llama_index.core import Document

# Ensure LLAMAPARSE_API_KEY and LLAMACLOUD_API_KEY are set in environment
os.environ["LLAMAPARSE_API_KEY"] = os.getenv("LLAMAPARSE_API_KEY")
os.environ["LLAMACLOUD_API_KEY"] = os.getenv("LLAMACLOUD_API_KEY")

# Use LlamaParse for robust PDF/Office document parsing (preserves structure)
parser = LlamaParseReader(result_type="markdown", parsing_instruction="Extract all tables and figures.")
documents: List[Document] = parser.load_data("path/to/complex_report.pdf")

# Store indexes in LlamaCloud for automatic scaling, versioning, and management
cloud_index = LlamaCloudIndex.from_documents(
    documents,
    name="my-production-rag-index",
    project_name="my-enterprise-rag",
    # auto_delete=False # Keep index after script finishes
)
query_engine = cloud_index.as_query_engine()
response = query_engine.query("Summarize the key findings in the report.")
print(response)
```

### ❌ 差:自定义爬取和用于复杂数据的基础读取器

```python
# Avoid custom web scraping when LlamaHub offers a robust connector.
# Avoid SimpleDirectoryReader for complex PDFs with tables/figures; it will lose structure.
from llama_index.core.readers import SimpleDirectoryReader
# docs = SimpleDirectoryReader("./complex_pdfs").load_data() # Leads to poor retrieval
```

## 3. 性能考虑

优化摄入、索引和检索以提高速度和降低成本。

### ✅ 好:异步操作和高效分块

```python
import asyncio
from llama_index.core import VectorStoreIndex, Document, Settings
from llama_index.core.node_parser import SentenceSplitter
from typing import List

async def build_index_async(documents: List[Document]) -> VectorStoreIndex:
    """Asynchronously builds a VectorStoreIndex."""
    # For very large datasets, consider streaming or batching document processing
    index = await asyncio.to_thread(VectorStoreIndex.from_documents, documents)
    return index

async def main_performance_example():
    Settings.text_splitter = SentenceSplitter(chunk_size=512, chunk_overlap=50)
    # Load documents (can be async if using async readers)
    docs = [Document(text=f"Sample document {i} content. " * 100) for i in range(100)]
    
    index = await build_index_async(docs)
    query_engine = index.as_query_engine()
    response = await asyncio.to_thread(query_engine.query, "What is in the documents?")
    print("Query completed:", response)

if __name__ == "__main__":
    asyncio.run(main_performance_example())
```

### ❌ 差:同步、未优化的处理

```python
# Synchronous processing of many documents is slow and blocks the event loop.
# Default chunking without consideration for content structure also hurts performance and quality.
from llama_index.core import SimpleDirectoryReader, VectorStoreIndex
# docs = SimpleDirectoryReader("./large_data_folder").load_data() # Can block
# index = VectorStoreIndex.from_documents(docs) # Can be memory intensive and slow
```

## 4. 常见陷阱和注意事项

### 4.1. 用于密钥的环境变量

始终使用环境变量存储 API 密钥和敏感凭证。

### ✅ 好:从环境加载

```python
import os
# Set these in your shell or .env file, e.g., OPENAI_API_KEY="sk-..."
os.environ["OPENAI_API_KEY"] = os.getenv("OPENAI_API_KEY")
os.environ["LLAMAPARSE_API_KEY"] = os.getenv("LLAMAPARSE_API_KEY")
```

### ❌ 差:硬编码密钥

```python
# NEVER hardcode API keys directly in code
OPENAI_API_KEY = "sk-YOUR_HARDCODED_KEY_HERE"
```

### 4.2. 分块策略

有效的分块对检索质量至关重要。

### ✅ 好:带元数据和重叠的语义分块

```python
from llama_index.core.node_parser import SentenceSplitter
from llama_index.core import Settings

# Configure chunking globally for consistency
Settings.text_splitter = SentenceSplitter(
    chunk_size=512,
    chunk_overlap=50,
    separator=" " # Prefer space separator for better word boundaries
)
# For complex documents, LlamaParse handles intelligent, structure-aware chunking.
```

### ❌ 差:默认或天真的分块

```python
# Relying solely on default chunking can lead to poor context and retrieval.
# Default is often fine for simple text, but not for structured documents or long texts.
# Settings.text_splitter = None # Don't explicitly disable if you need control
```

## 5. 类型提示

强制执行类型安全以实现可维护和健壮的 LlamaIndex 应用程序。

### ✅ 好:全面的类型注解

```python
from llama_index.core import Document, VectorStoreIndex
from llama_index.core.query_engine import BaseQueryEngine
from typing import List

def process_and_query_documents(documents: List[Document], query_str: str) -> str:
    """Processes documents and queries the index, returning the response as a string."""
    index: VectorStoreIndex = VectorStoreIndex.from_documents(documents)
    query_engine: BaseQueryEngine = index.as_query_engine()
    response = query_engine.query(query_str)
    return str(response)
```

### ❌ 差:无类型函数

```python
def process_and_query_untyped(documents, query_str): # Lacks clarity on expected types
    index = VectorStoreIndex.from_documents(documents)
    query_engine = index.as_query_engine()
    response = query_engine.query(query_str)
    return str(response)
```

## 6. 虚拟环境

隔离项目依赖项以防止冲突并确保可重现的构建。

### ✅ 好:使用 `venv` 或 `poetry`

```bash
# Using venv (standard Python approach)
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Using Poetry (recommended for robust dependency management)
poetry init --name my-llamaindex-app --python ">=3.9,<3.12"
poetry add llama-index llama-index-llms-openai llama-index-embeddings-openai
poetry shell
```

### ❌ 差:全局 `pip install`

```bash
# Avoid installing directly into your system's Python environment.
# This leads to dependency conflicts and "works on my machine" problems.
pip install llama-index # Can break other projects
```

## 7. 打包

清晰定义项目依赖项以实现可重现性和部署。

### ✅ 好:`pyproject.toml`(Poetry/Rye)或 `requirements.txt`

```toml
# pyproject.toml (Poetry example - preferred for modern Python projects)
[tool.poetry]
name = "my-llamaindex-app"
version = "0.1.0"
description = "A production-ready LlamaIndex RAG application."
authors = ["Your Name <you@example.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = ">=3.9,<3.12"
llama-index = "^0.10.0" # Use appropriate, pinned version ranges
llama-index-llms-openai = "^0.1.0"
llama-index-embeddings-openai = "^0.1.0"
# Add other necessary packages like llama-index-cloud, llama-index-readers-llamaparse
```

### ❌ 差:未记录的依赖项

```python
# No explicit dependency list makes it hard for others to set up the project.
# (Implicitly relying on a global environment or manual installs is a recipe for errors.)
```

## 8. 测试方法

实施单元测试和集成测试以确保可靠性。模拟 LLM 调用以实现更快、更便宜的单元测试。

### ✅ 好:带模拟的单元测试

```python
# tests/test_rag_components.py
import pytest
from unittest.mock import MagicMock
from llama_index.core import Document, Settings
from core_components import load_documents, build_vector_index, create_query_engine

@pytest.fixture
def mock_documents():
    return [
        Document(text="LlamaIndex is a data framework for LLM applications."),
        Document(text="It connects LLMs to your private data sources.")
    ]

def test_load_documents_from_dir(tmp_path):
    # Create dummy files for testing SimpleDirectoryReader
    (tmp_path / "doc1.txt").write_text("Content of document one.")
    (tmp_path / "doc2.txt").write_text("Content of document two.")
    docs = load_documents(str(tmp_path))
    assert len(docs) == 2
    assert "Content of document one." in docs[0].text

def test_build_vector_index(mock_documents):
    # Mock LLM and embedding models for fast, isolated unit tests
    Settings.llm = MagicMock()
    Settings.embed_model = MagicMock()
    index = build_vector_index(mock_documents)
    assert index is not None
    # Further assertions can check index properties if needed

def test_query_engine_response_mocked():
    mock_query_engine = MagicMock()
    mock_query_engine.query.return_value = "Mocked response: LlamaIndex connects LLMs to your data."
    response = mock_query_engine.query("What is LlamaIndex?")
    assert "Mocked response" in str(response)
    mock_query_engine.query.assert_called_once_with("What is LlamaIndex?")
```

### ❌ 差:无测试或仅依赖端到端测试

```python
# No test files, or only manual testing.
# Relying solely on slow, expensive end-to-end tests makes development cycles long and debugging difficult.
# Unit tests are crucial for isolating issues in specific components.
```