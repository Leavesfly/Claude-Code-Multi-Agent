---
description: Definitive guidelines for building high-performance, maintainable, and secure FastAPI applications using modern Python best practices.
---

# FastAPI 最佳实践

FastAPI 是高性能 Python API 的首选。本指南确保您的项目从第一天起就具有可扩展性、安全性和可维护性。

## 1. 代码组织：领域驱动的模块化

对于超出几个端点的任何项目，请按域功能而不是文件类型进行组织。这提高了可扩展性和团队协作。

❌ 不好：单个 `main.py` 或 `routers/`、`schemas/` 目录与所有域混合。
```python
# app/routers/users.py
# app/routers/items.py
# app/schemas/user.py
# app/schemas/item.py
```

✅ 好：按域对相关组件进行分组。
```
src/
├── auth/
│   ├── router.py
│   ├── schemas.py
│   ├── service.py # Business logic
│   └── dependencies.py
├── users/
│   ├── router.py
│   ├── schemas.py
│   ├── service.py
│   └── dependencies.py
├── core/
│   ├── config.py # Pydantic BaseSettings
│   └── security.py
├── db/
│   ├── session.py # SQLAlchemy engine/session
│   └── base.py    # Base for models
├── main.py        # Entry point
└── __init__.py
```

## 2. 类型提示：到处强制

利用 Python 的类型提示和 Pydantic 实现强大的数据验证、自动文档记录和 IDE 支持。

❌ 不好：类型提示缺失或不一致。
```python
@app.post("/items/")
def create_item(item: dict): # No Pydantic model
    return item
```

✅ 好：所有函数签名的显式 Pydantic 模型和类型提示。
```python
from pydantic import BaseModel
from fastapi import FastAPI

class ItemCreate(BaseModel):
    name: str
    description: str | None = None
    price: float

app = FastAPI()

@app.post("/items/", response_model=ItemCreate)
async def create_item(item: ItemCreate) -> ItemCreate:
    # Logic to save item
    return item
```

## 3. 依赖注入：解耦组件

使用 `fastapi.Depends` 来管理 database 会话、authentication 和其他共享资源。这使得代码可测试且模块化。

❌ 不好：全局 database session 或直接实例化。
```python
# In router.py
from app.db.session import SessionLocal
db = SessionLocal() # Global or directly called
```

✅ 好：使用 `Depends` 注入依赖项。
```python
from typing import Annotated
from fastapi import Depends
from sqlalchemy.ext.asyncio import AsyncSession
# Assume get_session and User are defined elsewhere
# from app.db.session import get_session
# from app.models.user import User

async def get_current_user(token: str) -> 'User': # 'User' for forward reference
    # ... auth logic
    return 'User'(id=1, username="test") # Placeholder

@app.get("/me/")
async def read_current_user(
    db: Annotated[AsyncSession, Depends(lambda: None)], # Placeholder for get_session
    current_user: Annotated['User', Depends(get_current_user)]
):
    return current_user
```

## 4. API 设计：版本控制和瘦端点

从第一天起就对您的 API 进行版本控制。集中 router 端点，将业务逻辑委托给服务层。

❌ 不好：未版本化的 API，具有业务逻辑的胖端点。
```python
# app/main.py
@app.get("/users/{user_id}")
def get_user_details(user_id: int, db: 'Session'): # Placeholder
    user = None # db.query(User).filter(User.id == user_id).first()
    # Complex business logic here
    return user
```

✅ 好：使用带有前缀和标签的“APIRouter”。将逻辑委托给“service.py”。
```python
# src/users/router.py
from fastapi import APIRouter, Depends
# Assume get_session, User, UserOut, service are defined elsewhere
# from src.users import service, schemas
# from app.db.session import get_session

router = APIRouter(prefix="/v1/users", tags=["Users"])

@router.get("/{user_id}", response_model=None) # Placeholder for schemas.UserOut
async def read_user(user_id: int, db: 'AsyncSession' = Depends(lambda: None)): # Placeholder
    user = None # await service.get_user_by_id(db, user_id)
    return user

# src/users/service.py
async def get_user_by_id(db: 'AsyncSession', user_id: int) -> 'User': # Placeholder
    # Database query logic
    return None # await db.get(User, user_id)
```

## 5. 错误处理：使用`HTTPException`

针对 API 特定错误引发 `HTTPException`。为全局错误类型实现自定义处理程序。

❌ 不好：引发通用 Python 异常。
```python
items_db = {1: {"name": "item1"}}
@app.get("/items/{item_id}")
async def get_item(item_id: int):
    if item_id not in items_db:
        raise ValueError("Item not found") # Returns 500
    return items_db[item_id]
```

✅ 好：使用适当的状态代码引发“HTTPException”。
```python
from fastapi import HTTPException, status

items_db = {1: {"name": "item1"}}
@app.get("/items/{item_id}")
async def get_item_good(item_id: int):
    if item_id not in items_db:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Item not found"
        )
    return items_db[item_id]
```

## 6. 性能：Async-首次和生产部署

对于 I/O 密集型操作，采用 `async`/`await`。对于 CPU 密集型任务，请使用“run_in_threadpool”。使用 Gunicorn + Uvicorn 进行部署。

❌ 不好：在 async 端点中阻塞 I/O。
```python
# In an async endpoint
import time
@app.get("/blocking")
async def blocking_endpoint():
    time.sleep(1) # Blocks the event loop
    return {"message": "Done blocking work"}
```

✅ 好：使用 async 库（例如，`asyncpg`、`httpx[async]`）或`run_in_threadpool`。
```python
from fastapi import FastAPI
from fastapi.concurrency import run_in_threadpool
import time

app = FastAPI()

def cpu_bound_task():
    time.sleep(0.1) # Simulate CPU work
    return "Done CPU work"

@app.get("/cpu-work")
async def handle_cpu_work():
    result = await run_in_threadpool(cpu_bound_task)
    return {"message": result}

# Production deployment:
# gunicorn -k uvicorn.workers.UvicornWorker src.main:app --workers 4 --bind 0.0.0.0:8000
```

## 7. 安全性：环境变量和身份验证

使用“pydantic-settings”将敏感配置存储在环境变量中。通过“Depends”实现 authentication。

❌ 不好：硬编码的秘密或配置。
```python
# app/core/config.py
DATABASE_URL = "postgresql://user:pass@host:port/db" # Bad
```

✅ 好：使用 `pydantic-settings` 进行基于环境的配置。
```python
# src/core/config.py
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    DATABASE_URL: str
    SECRET_KEY: str
    ALGORITHM: str = "HS256"

    model_config = SettingsConfigDict(env_file=".env", extra="ignore")

settings = Settings()
```
```python
# .env (local development)
DATABASE_URL="postgresql+asyncpg://user:pass@db:5432/app"
SECRET_KEY="your-super-secret-key"
```
```python
# src/core/security.py
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

async def get_current_active_user(token: str = Depends(oauth2_scheme)):
    # Validate token and return user
    return {"username": "current_user"} # Placeholder
```

## 8. 日志记录：结构化和集中化

结构化格式中的 Log 到 `stdout`/`stderr`（例如 JSON）。让您的部署环境处理聚合。

❌ 不好：将日志写入本地文件或非结构化打印语句。
```python
print("User accessed /health endpoint")
```

✅ 好：使用 Python 的 `logging` 模块和结构化格式化程序。
```python
import logging
# from pythonjsonlogger.jsonlogger import JsonFormatter # Install python-json-logger

LOGGING_CONFIG = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {
        "json": {
            "()": "jsonlogger.JsonFormatter", # Use if python-json-logger is installed
            "format": "%(levelname)s %(asctime)s %(name)s %(message)s"
        }
    },
    "handlers": {
        "default": {
            "formatter": "json", # Change to "standard" if jsonlogger not installed
            "class": "logging.StreamHandler",
            "stream": "ext://sys.stdout",
        },
    },
    "loggers": {
        "uvicorn": {"handlers": ["default"], "level": "INFO", "propagate": False},
        "uvicorn.access": {"handlers": ["default"], "level": "INFO", "propagate": False},
        "app": {"handlers": ["default"], "level": "INFO", "propagate": False},
    },
    "root": {"handlers": ["default"], "level": "INFO"},
}

logger = logging.getLogger("app")

@app.get("/health")
async def health():
    logger.info("Health check called", extra={"endpoint": "/health"})
    return {"status": "ok"}
```