<div align="center">

# Awesome FastAPI [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

⚡ **The Ultimate FastAPI Guide: Zero to Production Hero (2026 Edition)** - Build Lightning-Fast APIs!

<br>
<br>
</div>

Welcome to the most comprehensive FastAPI guide on the internet! This tutorial will transform you from a complete beginner to a FastAPI expert capable of building production-grade APIs. Whether you're building microservices, web applications, or ML APIs, this guide has everything you need.

**✨ What Makes This Guide Special:**
- **FastAPI 0.115+** with Python 3.12+ features
- **Zero to Production** - Complete journey with real examples
- **Modern Best Practices** - Type hints, async/await, dependency injection
- **Performance Focused** - One of the fastest Python frameworks
- **Production Patterns** - Authentication, testing, deployment, monitoring
- **Real-World Projects** - Build actual applications, not toy examples
- **Security First** - OAuth2, JWT, rate limiting, CORS

*Inspired by [awesome-python](https://github.com/vinta/awesome-python)* ✨


## 📋 Table of Contents

- **[Introduction](#-introduction)** - Why FastAPI Changes Everything
- **[Getting Started](#-getting-started)** - Setup & First API
- **[Core Concepts](#-core-concepts)** - Request/Response, Path Operations
- **[Request Handling](#-request-handling)** - Parameters, Body, Validation
- **[Response Models](#-response-models)** - Pydantic Models & Serialization
- **[Dependency Injection](#-dependency-injection)** - The FastAPI Secret Sauce
- **[Database Integration](#-database-integration)** - SQLAlchemy, MongoDB, PostgreSQL
- **[Authentication & Security](#-authentication--security)** - OAuth2, JWT, API Keys
- **[File Operations](#-file-operations)** - Upload, Download, Static Files
- **[Background Tasks](#-background-tasks)** - Async Tasks & Queues
- **[WebSockets](#-websockets)** - Real-Time Communication
- **[Testing](#-testing)** - Unit, Integration, E2E Testing
- **[Production Deployment](#-production-deployment)** - Docker, Kubernetes, CI/CD
- **[Advanced Topics](#-advanced-topics)** - Middleware, Events, GraphQL
- **[Resources](#-additional-resources)** - Learn More

---

## 🎯 Introduction

### What is FastAPI?

[FastAPI](https://fastapi.tiangolo.com/) is a **modern, blazing-fast** web framework for building APIs with Python 3.12+ based on standard Python type hints.

**The Elevator Pitch:**
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def read_root():
    return {"message": "Hello World"}

# That's it! You now have:
# ✅ Auto-generated OpenAPI docs at /docs
# ✅ Alternative docs at /redoc
# ✅ JSON Schema validation
# ✅ Editor support with autocomplete
# ✅ Async support out of the box
```

### Why FastAPI is a Game Changer

**🚀 Performance**
- On par with **NodeJS** and **Go**
- One of the fastest Python frameworks
- Built on **Starlette** (async) and **Pydantic** (validation)

**💡 Developer Experience**
- **Auto-generated documentation** (Swagger UI & ReDoc)
- **Type hints everywhere** - catch bugs before runtime
- **Editor support** - autocomplete, error checking
- **Intuitive API** - easy to learn, hard to mess up

**🛡️ Production Ready**
- **Built-in security** utilities (OAuth2, JWT)
- **Data validation** with Pydantic models
- **Dependency injection** system
- **Async support** for high concurrency

**📊 Industry Adoption:**
```
Companies using FastAPI:
🏢 Microsoft     🏢 Uber
🏢 Netflix       🏢 Revolut
🏢 Expedia       🏢 Cisco
... and thousands more!
```

### FastAPI vs The Competition

| Feature | FastAPI | Flask | Django REST | Express.js |
|---------|---------|-------|-------------|------------|
| Speed | ⚡⚡⚡⚡⚡ | ⚡⚡ | ⚡⚡⚡ | ⚡⚡⚡⚡ |
| Async Native | ✅ | ❌ | ❌ | ✅ |
| Auto Docs | ✅ | ❌ | ✅ | ❌ |
| Type Safety | ✅ | ❌ | ❌ | With TS |
| Learning Curve | Easy | Easy | Medium | Easy |
| Data Validation | ✅ Built-in | Manual | DRF Serializers | Manual |
| DI System | ✅ | ❌ | ❌ | ❌ |

**Latest Version:** FastAPI 0.115+
- ✅ Python 3.12+ support (with 3.13 optimizations!)
- ✅ Pydantic V2 integration
- ✅ Enhanced WebSocket support
- ✅ Improved async performance
- ✅ Better OpenAPI 3.1 support


## 🚀 Getting Started

### Prerequisites

- [ ] Python 3.12 or higher (3.13 recommended for performance!)
- [ ] Basic Python knowledge
- [ ] Understanding of HTTP/REST APIs
- [ ] Code editor (VS Code recommended)

### Installation

```bash
# Create project directory
mkdir awesome-fastapi-project
cd awesome-fastapi-project

# Create virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate     # Windows

# Install FastAPI and Uvicorn
pip install "fastapi[standard]"

# Or install separately
pip install fastapi
pip install "uvicorn[standard]"

# Save dependencies
pip freeze > requirements.txt
```

**What's Included:**
- `fastapi` - The framework
- `uvicorn` - ASGI server (production-ready)
- `pydantic` - Data validation
- `starlette` - Low-level components
- `python-multipart` - For file uploads
- `email-validator` - Email validation
- `ujson` - Faster JSON parsing
- `uvloop` - Faster async loop (Unix only)
- `httptools` - Faster HTTP parsing

### Your First API (Hello World!)

Create `main.py`:

```python
from fastapi import FastAPI

# Create FastAPI instance
app = FastAPI(
    title="My Awesome API",
    description="This is my first FastAPI project!",
    version="1.0.0"
)

# Define a route
@app.get("/")
async def root():
    """Root endpoint - returns a greeting."""
    return {"message": "Hello World!"}

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    """Get an item by ID."""
    return {"item_id": item_id, "name": f"Item {item_id}"}

# Run with: uvicorn main:app --reload
```

**Run the server:**

```bash
# Development mode (auto-reload on code changes)
uvicorn main:app --reload

# Specify host and port
uvicorn main:app --host 0.0.0.0 --port 8000 --reload

# With log level
uvicorn main:app --reload --log-level debug
```

**Test your API:**

```bash
# Open in browser
http://localhost:8000

# Auto-generated docs (Swagger UI)
http://localhost:8000/docs

# Alternative docs (ReDoc)
http://localhost:8000/redoc

# OpenAPI JSON schema
http://localhost:8000/openapi.json

# Test with curl
curl http://localhost:8000
curl http://localhost:8000/items/42
```

**🎉 Congratulations!** You just:
- ✅ Created a FastAPI application
- ✅ Defined two endpoints
- ✅ Got auto-generated interactive docs
- ✅ Validated path parameters automatically


## 🔧 Core Concepts

### Path Operations

Path operations are the heart of your API - they define routes and HTTP methods.

```python
from fastapi import FastAPI

app = FastAPI()

# GET request
@app.get("/users")
async def get_users():
    """Retrieve list of users."""
    return {"users": ["Alice", "Bob", "Charlie"]}

# POST request
@app.post("/users")
async def create_user():
    """Create a new user."""
    return {"message": "User created"}

# PUT request
@app.put("/users/{user_id}")
async def update_user(user_id: int):
    """Update an existing user."""
    return {"user_id": user_id, "message": "User updated"}

# DELETE request
@app.delete("/users/{user_id}")
async def delete_user(user_id: int):
    """Delete a user."""
    return {"user_id": user_id, "message": "User deleted"}

# PATCH request
@app.patch("/users/{user_id}")
async def partial_update_user(user_id: int):
    """Partially update a user."""
    return {"user_id": user_id, "message": "User partially updated"}

# Multiple HTTP methods
from fastapi import Request

@app.api_route("/items/{item_id}", methods=["GET", "POST"])
async def handle_item(item_id: int, request: Request):
    """Handle both GET and POST for items."""
    if request.method == "GET":
        return {"item_id": item_id, "action": "read"}
    else:
        return {"item_id": item_id, "action": "create"}
```

### Path Parameters

Extract values from the URL path:

```python
from fastapi import FastAPI, Path

app = FastAPI()

# Basic path parameter
@app.get("/users/{user_id}")
async def get_user(user_id: int):
    """Type is automatically validated!"""
    return {"user_id": user_id}

# Multiple path parameters
@app.get("/users/{user_id}/posts/{post_id}")
async def get_user_post(user_id: int, post_id: int):
    return {"user_id": user_id, "post_id": post_id}

# Path parameter with validation
@app.get("/items/{item_id}")
async def get_item(
    item_id: int = Path(
        ...,  # Required
        title="Item ID",
        description="The ID of the item to retrieve",
        ge=1,  # Greater than or equal to 1
        le=1000  # Less than or equal to 1000
    )
):
    return {"item_id": item_id}

# String path parameter with pattern
@app.get("/files/{file_path:path}")
async def get_file(file_path: str):
    """Capture the entire path."""
    return {"file_path": file_path}

# Enum path parameter
from enum import Enum

class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    """Path parameter with predefined values."""
    if model_name == ModelName.alexnet:
        return {"model": "AlexNet", "message": "Deep Learning FTW!"}
    elif model_name == ModelName.lenet:
        return {"model": "LeNet", "message": "LeCNN"}
    return {"model": model_name, "message": "Have some residuals"}
```

### Query Parameters

Extract values from URL query strings:

```python
from fastapi import FastAPI, Query
from typing import Optional

app = FastAPI()

# Basic query parameters
@app.get("/items")
async def read_items(skip: int = 0, limit: int = 10):
    """
    /items?skip=0&limit=10
    """
    return {"skip": skip, "limit": limit}

# Optional query parameters
@app.get("/users")
async def read_users(
    q: Optional[str] = None,
    active: bool = False
):
    """
    /users?q=alice&active=true
    """
    result = {"active": active}
    if q:
        result["q"] = q
    return result

# Query parameter with validation
@app.get("/search")
async def search_items(
    q: str = Query(
        ...,  # Required
        min_length=3,
        max_length=50,
        pattern="^[a-zA-Z0-9 ]+$",  # Regex validation
        title="Query string",
        description="Search query for items"
    )
):
    return {"q": q}

# Multiple values (list)
@app.get("/tags")
async def read_tags(tags: list[str] = Query(default=[])):
    """
    /tags?tags=python&tags=fastapi&tags=awesome
    """
    return {"tags": tags}

# Default list
@app.get("/items-list")
async def read_items_list(
    tags: list[str] = Query(default=["foo", "bar"])
):
    return {"tags": tags}

# Numeric validations
@app.get("/products")
async def read_products(
    price_min: float = Query(0, ge=0),
    price_max: float = Query(1000, le=10000),
    rating: float = Query(0, ge=0, le=5)
):
    return {
        "price_min": price_min,
        "price_max": price_max,
        "rating": rating
    }
```

### Request Body

Handle JSON request bodies with Pydantic models:

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field, EmailStr
from typing import Optional
from datetime import datetime

app = FastAPI()

# Define Pydantic model
class User(BaseModel):
    username: str = Field(..., min_length=3, max_length=50)
    email: EmailStr
    full_name: Optional[str] = None
    age: Optional[int] = Field(None, ge=0, le=150)
    is_active: bool = True
    created_at: datetime = Field(default_factory=datetime.now)

# Use model in endpoint
@app.post("/users")
async def create_user(user: User):
    """
    Send POST request with JSON body:
    {
        "username": "alice",
        "email": "alice@example.com",
        "full_name": "Alice Wonder",
        "age": 28
    }
    """
    return {
        "message": "User created",
        "user": user
    }

# Nested models
class Address(BaseModel):
    street: str
    city: str
    country: str = "USA"
    zip_code: Optional[str] = None

class UserWithAddress(BaseModel):
    username: str
    email: EmailStr
    address: Address  # Nested model

@app.post("/users-with-address")
async def create_user_with_address(user: UserWithAddress):
    return user

# List of models
@app.post("/users/batch")
async def create_users_batch(users: list[User]):
    """
    Send array of users:
    [
        {"username": "alice", "email": "alice@example.com"},
        {"username": "bob", "email": "bob@example.com"}
    ]
    """
    return {"count": len(users), "users": users}

# Mixing path, query, and body parameters
@app.put("/users/{user_id}")
async def update_user(
    user_id: int,
    user: User,
    importance: int = Query(default=0, ge=0, le=10)
):
    return {
        "user_id": user_id,
        "importance": importance,
        "user": user
    }
```

### Async vs Sync

FastAPI supports both sync and async functions:

```python
import time
import asyncio

# Async function (preferred for I/O operations)
@app.get("/async-endpoint")
async def async_endpoint():
    """Use async for I/O-bound operations."""
    await asyncio.sleep(1)  # Non-blocking sleep
    return {"message": "Async response"}

# Sync function (for CPU-bound operations)
@app.get("/sync-endpoint")
def sync_endpoint():
    """Use sync for CPU-bound operations."""
    time.sleep(1)  # Blocking sleep (runs in thread pool)
    return {"message": "Sync response"}

# When to use async:
# ✅ Database queries
# ✅ HTTP requests to external APIs
# ✅ File I/O operations
# ✅ WebSocket connections

# When to use sync:
# ✅ CPU-intensive computations
# ✅ When using sync-only libraries
# ✅ Simple operations without I/O

# Example: Database operation (async)
@app.get("/users/{user_id}")
async def get_user_from_db(user_id: int):
    # Async database query
    user = await db.fetch_one("SELECT * FROM users WHERE id = %s", user_id)
    return user

# Example: Heavy computation (sync)
@app.post("/calculate")
def heavy_calculation(data: dict):
    # CPU-intensive operation runs in thread pool
    result = perform_complex_calculation(data)
    return {"result": result}
```

**🎯 Pro Tip:** Use `async def` by default! FastAPI will handle it efficiently even if you don't use `await`.


## 📥 Request Handling

### Headers

Access request headers:

```python
from fastapi import FastAPI, Header
from typing import Optional

app = FastAPI()

# Basic header
@app.get("/items")
async def read_items(user_agent: Optional[str] = Header(None)):
    """Access User-Agent header."""
    return {"User-Agent": user_agent}

# Custom header with validation
@app.get("/api/data")
async def get_data(
    x_api_key: str = Header(..., description="API Key for authentication"),
    x_request_id: Optional[str] = Header(None)
):
    return {
        "api_key": x_api_key,
        "request_id": x_request_id
    }

# Multiple headers
@app.get("/info")
async def get_info(
    host: str = Header(...),
    accept: str = Header(...),
    accept_encoding: str = Header(...),
    user_agent: str = Header(...)
):
    return {
        "host": host,
        "accept": accept,
        "accept_encoding": accept_encoding,
        "user_agent": user_agent
    }

# Convert header names (HTTP headers are case-insensitive)
@app.get("/convert")
async def header_convert(
    # HTTP: X-Custom-Header
    # Python: x_custom_header (underscores converted to hyphens)
    x_custom_header: str = Header(...)
):
    return {"x_custom_header": x_custom_header}
```

### Cookies

Handle cookies:

```python
from fastapi import FastAPI, Cookie, Response
from typing import Optional

app = FastAPI()

# Read cookie
@app.get("/items")
async def read_items(session_id: Optional[str] = Cookie(None)):
    return {"session_id": session_id}

# Set cookie
@app.post("/login")
async def login(response: Response):
    # Set cookie in response
    response.set_cookie(
        key="session_id",
        value="abc123",
        max_age=3600,  # 1 hour
        httponly=True,  # Prevent XSS
        secure=True,    # HTTPS only
        samesite="lax"  # CSRF protection
    )
    return {"message": "Logged in"}

# Delete cookie
@app.post("/logout")
async def logout(response: Response):
    response.delete_cookie("session_id")
    return {"message": "Logged out"}
```

### Forms

Handle form data:

```bash
# Install python-multipart
pip install python-multipart
```

```python
from fastapi import FastAPI, Form

app = FastAPI()

# Simple form
@app.post("/login")
async def login(
    username: str = Form(...),
    password: str = Form(...)
):
    return {"username": username}

# Form with validation
@app.post("/register")
async def register(
    username: str = Form(..., min_length=3, max_length=50),
    email: str = Form(..., regex=r"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"),
    password: str = Form(..., min_length=8),
    age: int = Form(..., ge=13, le=120)
):
    return {
        "username": username,
        "email": email,
        "age": age
    }

# Mixed form and files (we'll cover files later)
from fastapi import File, UploadFile

@app.post("/profile")
async def create_profile(
    username: str = Form(...),
    bio: str = Form(...),
    avatar: UploadFile = File(...)
):
    return {
        "username": username,
        "bio": bio,
        "filename": avatar.filename
    }
```

### Request Object

Access the raw request:

```python
from fastapi import FastAPI, Request
import json

app = FastAPI()

@app.post("/analyze")
async def analyze_request(request: Request):
    """Access complete request object."""
    return {
        "method": request.method,
        "url": str(request.url),
        "headers": dict(request.headers),
        "query_params": dict(request.query_params),
        "path_params": dict(request.path_params),
        "client_host": request.client.host if request.client else None,
        "cookies": request.cookies,
    }

# Get raw body
@app.post("/raw-body")
async def get_raw_body(request: Request):
    body = await request.body()
    return {"raw_body": body.decode()}

# Get JSON manually
@app.post("/manual-json")
async def manual_json(request: Request):
    json_data = await request.json()
    return json_data
```


## 📤 Response Models

### Pydantic Models

Define response structure with Pydantic:

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field
from typing import Optional, List
from datetime import datetime

app = FastAPI()

# Response model
class UserResponse(BaseModel):
    id: int
    username: str
    email: str
    created_at: datetime
    is_active: bool = True
    
    class Config:
        # Allow ORM models
        from_attributes = True

# Use response model
@app.get("/users/{user_id}", response_model=UserResponse)
async def get_user(user_id: int):
    """
    Response will be validated and serialized
    according to UserResponse model.
    """
    return {
        "id": user_id,
        "username": "alice",
        "email": "alice@example.com",
        "created_at": datetime.now(),
        "is_active": True,
        "password": "secret123"  # This will be filtered out!
    }

# List response
@app.get("/users", response_model=List[UserResponse])
async def list_users():
    return [
        {"id": 1, "username": "alice", "email": "alice@example.com", "created_at": datetime.now()},
        {"id": 2, "username": "bob", "email": "bob@example.com", "created_at": datetime.now()}
    ]

# Nested response models
class Post(BaseModel):
    id: int
    title: str
    content: str
    author: UserResponse  # Nested model

@app.get("/posts/{post_id}", response_model=Post)
async def get_post(post_id: int):
    return {
        "id": post_id,
        "title": "My Post",
        "content": "Content here...",
        "author": {
            "id": 1,
            "username": "alice",
            "email": "alice@example.com",
            "created_at": datetime.now()
        }
    }
```

### Response Model Features

```python
from pydantic import BaseModel
from typing import Optional

# Model with optional fields
class UserInDB(BaseModel):
    id: int
    username: str
    email: str
    hashed_password: str  # Sensitive!
    is_active: bool = True

class UserPublic(BaseModel):
    id: int
    username: str
    email: str
    is_active: bool = True

# Exclude fields from response
@app.get("/users/{user_id}", response_model=UserPublic)
async def get_user_public(user_id: int):
    # Database returns UserInDB
    user_in_db = {
        "id": user_id,
        "username": "alice",
        "email": "alice@example.com",
        "hashed_password": "$2b$12$abc123...",  # Won't be in response!
        "is_active": True
    }
    return user_in_db

# Response model with exclude
@app.get(
    "/users/{user_id}/admin",
    response_model=UserInDB,
    response_model_exclude={"hashed_password"}  # Exclude specific fields
)
async def get_user_admin(user_id: int):
    return get_user_from_db(user_id)

# Include only specific fields
@app.get(
    "/users/{user_id}/minimal",
    response_model=UserInDB,
    response_model_include={"id", "username"}  # Only these fields
)
async def get_user_minimal(user_id: int):
    return get_user_from_db(user_id)

# Exclude unset fields (don't send null values)
@app.get(
    "/users/{user_id}",
    response_model=UserPublic,
    response_model_exclude_unset=True
)
async def get_user_no_nulls(user_id: int):
    return {"id": user_id, "username": "alice"}
    # email and is_active won't be in response (not set)
```

### Status Codes

Return different status codes:

```python
from fastapi import FastAPI, status, Response, HTTPException

app = FastAPI()

# Explicit status code
@app.post("/users", status_code=status.HTTP_201_CREATED)
async def create_user(user: User):
    """Returns 201 Created instead of default 200."""
    return {"id": 1, "username": user.username}

# Dynamic status code
@app.put("/items/{item_id}")
async def update_item(item_id: int, response: Response):
    if item_id > 1000:
        response.status_code = status.HTTP_201_CREATED
        return {"message": "Item created"}
    else:
        response.status_code = status.HTTP_200_OK
        return {"message": "Item updated"}

# Common status codes
@app.delete("/items/{item_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_item(item_id: int):
    """Returns 204 No Content (empty response)."""
    return None

# Error status codes
@app.get("/items/{item_id}")
async def get_item(item_id: int):
    if item_id > 1000:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Item not found"
        )
    return {"item_id": item_id}

# Multiple response status codes documentation
from typing import Union

@app.get(
    "/items/{item_id}",
    responses={
        200: {"description": "Item found"},
        404: {"description": "Item not found"},
        500: {"description": "Internal server error"}
    }
)
async def get_item_docs(item_id: int):
    return {"item_id": item_id}
```

### Custom Responses

Return different response types:

```python
from fastapi import FastAPI
from fastapi.responses import (
    JSONResponse,
    HTMLResponse,
    PlainTextResponse,
    RedirectResponse,
    FileResponse,
    StreamingResponse
)

app = FastAPI()

# JSON Response (default)
@app.get("/json")
async def get_json():
    return {"message": "This is JSON"}

# HTML Response
@app.get("/html", response_class=HTMLResponse)
async def get_html():
    html_content = """
    <html>
        <head><title>FastAPI</title></head>
        <body>
            <h1>Hello from FastAPI!</h1>
        </body>
    </html>
    """
    return html_content

# Plain Text Response
@app.get("/text", response_class=PlainTextResponse)
async def get_text():
    return "Hello, this is plain text!"

# Redirect Response
@app.get("/redirect")
async def redirect():
    return RedirectResponse(url="/docs")

# Custom JSON Response with headers
@app.get("/custom-json")
async def custom_json():
    content = {"message": "Custom JSON"}
    headers = {"X-Custom-Header": "CustomValue"}
    return JSONResponse(content=content, headers=headers)

# File Response
@app.get("/download")
async def download_file():
    return FileResponse(
        path="files/document.pdf",
        filename="my-document.pdf",
        media_type="application/pdf"
    )

# Streaming Response
import io

@app.get("/stream")
async def stream_data():
    def generate():
        for i in range(10):
            yield f"Line {i}\n"
    
    return StreamingResponse(
        generate(),
        media_type="text/plain"
    )
```


## 💉 Dependency Injection

Dependency Injection is FastAPI's **secret weapon** for writing clean, reusable code!

### Basic Dependencies

```python
from fastapi import FastAPI, Depends
from typing import Optional

app = FastAPI()

# Simple dependency
def common_parameters(
    skip: int = 0,
    limit: int = 10,
    q: Optional[str] = None
):
    """Reusable dependency for pagination and search."""
    return {"skip": skip, "limit": limit, "q": q}

# Use dependency in multiple endpoints
@app.get("/items")
async def read_items(commons: dict = Depends(common_parameters)):
    """
    /items?skip=0&limit=10&q=search
    """
    return commons

@app.get("/users")
async def read_users(commons: dict = Depends(common_parameters)):
    return commons

# Class-based dependency
class CommonQueryParams:
    def __init__(
        self,
        skip: int = 0,
        limit: int = 100,
        q: Optional[str] = None
    ):
        self.skip = skip
        self.limit = limit
        self.q = q

@app.get("/products")
async def read_products(commons: CommonQueryParams = Depends()):
    """
    Depends() without arguments uses the type hint!
    """
    return {
        "skip": commons.skip,
        "limit": commons.limit,
        "q": commons.q
    }
```

### Sub-dependencies

Dependencies can depend on other dependencies!

```python
from fastapi import FastAPI, Depends, Header, HTTPException

app = FastAPI()

# First dependency
def verify_token(x_token: str = Header(...)):
    """Verify authentication token."""
    if x_token != "secret-token":
        raise HTTPException(status_code=401, detail="Invalid token")
    return x_token

# Second dependency (depends on first)
def verify_key(
    x_key: str = Header(...),
    token: str = Depends(verify_token)  # Sub-dependency!
):
    """Verify API key (requires valid token)."""
    if x_key != "secret-key":
        raise HTTPException(status_code=403, detail="Invalid key")
    return x_key

# Use sub-dependencies
@app.get("/protected")
async def protected_route(key: str = Depends(verify_key)):
    """Requires both valid token AND key."""
    return {"message": "Access granted", "key": key}
```

### Database Dependencies

```python
from fastapi import FastAPI, Depends
from sqlalchemy.orm import Session

# Database session dependency
def get_db():
    """Provide database session."""
    db = SessionLocal()
    try:
        yield db  # Yield the session
    finally:
        db.close()  # Always close after use

# Use in endpoints
@app.get("/users/{user_id}")
async def get_user(
    user_id: int,
    db: Session = Depends(get_db)
):
    """Database session automatically provided and closed."""
    user = db.query(User).filter(User.id == user_id).first()
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user

@app.post("/users")
async def create_user(
    user: UserCreate,
    db: Session = Depends(get_db)
):
    db_user = User(**user.dict())
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user
```

### Global Dependencies

Apply dependencies to all routes:

```python
from fastapi import FastAPI, Depends, Header, HTTPException

# Authentication dependency
async def verify_api_key(x_api_key: str = Header(...)):
    if x_api_key != "secret-api-key":
        raise HTTPException(status_code=403, detail="Invalid API Key")
    return x_api_key

# Apply to all routes
app = FastAPI(dependencies=[Depends(verify_api_key)])

# Now ALL routes require valid API key
@app.get("/items")
async def get_items():
    return {"items": ["item1", "item2"]}

@app.get("/users")
async def get_users():
    return {"users": ["alice", "bob"]}

# Or apply to router
from fastapi import APIRouter

router = APIRouter(
    prefix="/admin",
    dependencies=[Depends(verify_api_key)]  # All /admin routes protected
)

@router.get("/dashboard")
async def admin_dashboard():
    return {"dashboard": "admin"}
```

### Dependency Caching

Dependencies are cached per request:

```python
from fastapi import FastAPI, Depends

app = FastAPI()

# This dependency will be called only ONCE per request
def expensive_operation():
    print("🔥 Expensive operation running...")
    # Simulate expensive operation
    result = perform_heavy_computation()
    return result

@app.get("/route1")
async def route1(
    data1: dict = Depends(expensive_operation),
    data2: dict = Depends(expensive_operation)  # Uses cached result!
):
    """
    expensive_operation() runs only ONCE,
    even though it's used twice!
    """
    return {"data1": data1, "data2": data2}

# Disable caching if needed
@app.get("/route2")
async def route2(
    data: dict = Depends(expensive_operation, use_cache=False)
):
    """Now it runs every time."""
    return data
```


## 🗄️ Database Integration

### SQLAlchemy Setup (Relational DBs)

```bash
pip install sqlalchemy psycopg2-binary
```

**database.py:**
```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# Database URL
SQLALCHEMY_DATABASE_URL = "postgresql://user:password@localhost/dbname"
# For SQLite: "sqlite:///./app.db"

# Create engine
engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    # SQLite only: connect_args={"check_same_thread": False}
)

# Session factory
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

# Base class for models
Base = declarative_base()

# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

**models.py:**
```python
from sqlalchemy import Boolean, Column, Integer, String, DateTime, ForeignKey
from sqlalchemy.orm import relationship
from database import Base
from datetime import datetime

class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True, index=True)
    email = Column(String, unique=True, index=True, nullable=False)
    username = Column(String, unique=True, index=True, nullable=False)
    hashed_password = Column(String, nullable=False)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # Relationship
    posts = relationship("Post", back_populates="owner")

class Post(Base):
    __tablename__ = "posts"
    
    id = Column(Integer, primary_key=True, index=True)
    title = Column(String, index=True)
    content = Column(String)
    owner_id = Column(Integer, ForeignKey("users.id"))
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # Relationship
    owner = relationship("User", back_populates="posts")
```

**schemas.py:**
```python
from pydantic import BaseModel, EmailStr
from datetime import datetime
from typing import Optional, List

# User schemas
class UserBase(BaseModel):
    email: EmailStr
    username: str

class UserCreate(UserBase):
    password: str

class UserUpdate(BaseModel):
    email: Optional[EmailStr] = None
    username: Optional[str] = None
    password: Optional[str] = None

class User(UserBase):
    id: int
    is_active: bool
    created_at: datetime
    
    class Config:
        from_attributes = True

# Post schemas
class PostBase(BaseModel):
    title: str
    content: str

class PostCreate(PostBase):
    pass

class Post(PostBase):
    id: int
    owner_id: int
    created_at: datetime
    owner: User  # Nested user
    
    class Config:
        from_attributes = True
```

**crud.py:**
```python
from sqlalchemy.orm import Session
import models, schemas
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

# User CRUD
def get_user(db: Session, user_id: int):
    return db.query(models.User).filter(models.User.id == user_id).first()

def get_user_by_email(db: Session, email: str):
    return db.query(models.User).filter(models.User.email == email).first()

def get_users(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.User).offset(skip).limit(limit).all()

def create_user(db: Session, user: schemas.UserCreate):
    hashed_password = pwd_context.hash(user.password)
    db_user = models.User(
        email=user.email,
        username=user.username,
        hashed_password=hashed_password
    )
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

def update_user(db: Session, user_id: int, user: schemas.UserUpdate):
    db_user = get_user(db, user_id)
    if not db_user:
        return None
    
    update_data = user.dict(exclude_unset=True)
    if "password" in update_data:
        update_data["hashed_password"] = pwd_context.hash(update_data.pop("password"))
    
    for field, value in update_data.items():
        setattr(db_user, field, value)
    
    db.commit()
    db.refresh(db_user)
    return db_user

def delete_user(db: Session, user_id: int):
    db_user = get_user(db, user_id)
    if db_user:
        db.delete(db_user)
        db.commit()
    return db_user

# Post CRUD
def create_post(db: Session, post: schemas.PostCreate, user_id: int):
    db_post = models.Post(**post.dict(), owner_id=user_id)
    db.add(db_post)
    db.commit()
    db.refresh(db_post)
    return db_post

def get_posts(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.Post).offset(skip).limit(limit).all()
```

**main.py:**
```python
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.orm import Session
from typing import List

import models, schemas, crud
from database import engine, get_db

# Create tables
models.Base.metadata.create_all(bind=engine)

app = FastAPI(title="User & Posts API")

# User endpoints
@app.post("/users", response_model=schemas.User, status_code=201)
async def create_user(
    user: schemas.UserCreate,
    db: Session = Depends(get_db)
):
    """Create a new user."""
    db_user = crud.get_user_by_email(db, email=user.email)
    if db_user:
        raise HTTPException(status_code=400, detail="Email already registered")
    return crud.create_user(db=db, user=user)

@app.get("/users", response_model=List[schemas.User])
async def read_users(
    skip: int = 0,
    limit: int = 100,
    db: Session = Depends(get_db)
):
    """Get list of users."""
    users = crud.get_users(db, skip=skip, limit=limit)
    return users

@app.get("/users/{user_id}", response_model=schemas.User)
async def read_user(user_id: int, db: Session = Depends(get_db)):
    """Get user by ID."""
    db_user = crud.get_user(db, user_id=user_id)
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return db_user

@app.put("/users/{user_id}", response_model=schemas.User)
async def update_user(
    user_id: int,
    user: schemas.UserUpdate,
    db: Session = Depends(get_db)
):
    """Update user."""
    db_user = crud.update_user(db, user_id=user_id, user=user)
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return db_user

@app.delete("/users/{user_id}")
async def delete_user(user_id: int, db: Session = Depends(get_db)):
    """Delete user."""
    db_user = crud.delete_user(db, user_id=user_id)
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return {"message": "User deleted"}

# Post endpoints
@app.post("/users/{user_id}/posts", response_model=schemas.Post, status_code=201)
async def create_post_for_user(
    user_id: int,
    post: schemas.PostCreate,
    db: Session = Depends(get_db)
):
    """Create post for user."""
    return crud.create_post(db=db, post=post, user_id=user_id)

@app.get("/posts", response_model=List[schemas.Post])
async def read_posts(
    skip: int = 0,
    limit: int = 100,
    db: Session = Depends(get_db)
):
    """Get list of posts."""
    posts = crud.get_posts(db, skip=skip, limit=limit)
    return posts
```

### MongoDB Integration

```bash
pip install motor  # Async MongoDB driver
```

**database.py:**
```python
from motor.motor_asyncio import AsyncIOMotorClient
from fastapi import FastAPI

# MongoDB connection
MONGODB_URL = "mongodb://localhost:27017"
DATABASE_NAME = "myapp"

# Global client
client = None

async def connect_to_mongo():
    """Connect to MongoDB on startup."""
    global client
    client = AsyncIOMotorClient(MONGODB_URL)
    print("✅ Connected to MongoDB")

async def close_mongo_connection():
    """Close MongoDB connection on shutdown."""
    global client
    if client:
        client.close()
        print("❌ Closed MongoDB connection")

def get_database():
    """Get MongoDB database."""
    return client[DATABASE_NAME]
```

**main.py:**
```python
from fastapi import FastAPI, HTTPException, Depends
from pydantic import BaseModel, Field
from typing import List, Optional
from datetime import datetime
from bson import ObjectId

app = FastAPI()

# Connect on startup
@app.on_event("startup")
async def startup_db_client():
    await connect_to_mongo()

@app.on_event("shutdown")
async def shutdown_db_client():
    await close_mongo_connection()

# Pydantic models
class PyObjectId(ObjectId):
    @classmethod
    def __get_validators__(cls):
        yield cls.validate
    
    @classmethod
    def validate(cls, v):
        if not ObjectId.is_valid(v):
            raise ValueError("Invalid ObjectId")
        return ObjectId(v)

class UserModel(BaseModel):
    id: Optional[PyObjectId] = Field(alias="_id")
    name: str
    email: str
    created_at: datetime = Field(default_factory=datetime.utcnow)
    
    class Config:
        populate_by_name = True
        arbitrary_types_allowed = True
        json_encoders = {ObjectId: str}

# Dependency
def get_users_collection():
    db = get_database()
    return db.users

# Endpoints
@app.post("/users", response_model=UserModel, status_code=201)
async def create_user(
    user: UserModel,
    collection = Depends(get_users_collection)
):
    """Create user in MongoDB."""
    user_dict = user.dict(by_alias=True, exclude={"id"})
    result = await collection.insert_one(user_dict)
    user_dict["_id"] = result.inserted_id
    return user_dict

@app.get("/users", response_model=List[UserModel])
async def list_users(
    skip: int = 0,
    limit: int = 100,
    collection = Depends(get_users_collection)
):
    """List users."""
    users = await collection.find().skip(skip).limit(limit).to_list(length=limit)
    return users

@app.get("/users/{user_id}", response_model=UserModel)
async def get_user(
    user_id: str,
    collection = Depends(get_users_collection)
):
    """Get user by ID."""
    user = await collection.find_one({"_id": ObjectId(user_id)})
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```


## 🔐 Authentication & Security

### Password Hashing

```bash
pip install passlib bcrypt python-jose[cryptography]
```

```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str) -> str:
    """Hash a password."""
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """Verify password against hash."""
    return pwd_context.verify(plain_password, hashed_password)
```

### JWT Authentication

```python
from datetime import datetime, timedelta
from typing import Optional
from jose import JWTError, jwt
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm

# Configuration
SECRET_KEY = "your-secret-key-keep-it-secret"  # Use environment variable!
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

def create_access_token(data: dict, expires_delta: Optional[timedelta] = None):
    """Create JWT access token."""
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=15)
    
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

async def get_current_user(token: str = Depends(oauth2_scheme)):
    """Get current user from JWT token."""
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise credentials_exception
    except JWTError:
        raise credentials_exception
    
    # Get user from database
    user = get_user_by_username(username)
    if user is None:
        raise credentials_exception
    return user

# Login endpoint
@app.post("/token")
async def login(form_data: OAuth2PasswordRequestForm = Depends()):
    """Login and get access token."""
    user = authenticate_user(form_data.username, form_data.password)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = create_access_token(
        data={"sub": user.username},
        expires_delta=access_token_expires
    )
    return {"access_token": access_token, "token_type": "bearer"}

# Protected route
@app.get("/users/me")
async def read_users_me(current_user: User = Depends(get_current_user)):
    """Get current user info."""
    return current_user

# Require active user
async def get_current_active_user(
    current_user: User = Depends(get_current_user)
):
    """Ensure user is active."""
    if not current_user.is_active:
        raise HTTPException(status_code=400, detail="Inactive user")
    return current_user

@app.get("/users/me/items")
async def read_own_items(
    current_user: User = Depends(get_current_active_user)
):
    return {"owner": current_user.username, "items": [...]}
```

### API Key Authentication

```python
from fastapi import Security, HTTPException, status
from fastapi.security import APIKeyHeader

API_KEY_NAME = "X-API-Key"
api_key_header = APIKeyHeader(name=API_KEY_NAME, auto_error=False)

async def get_api_key(api_key: str = Security(api_key_header)):
    """Validate API key."""
    if api_key == "your-secret-api-key":  # Check against database
        return api_key
    raise HTTPException(
        status_code=status.HTTP_403_FORBIDDEN,
        detail="Invalid API Key"
    )

# Protected route
@app.get("/protected")
async def protected_route(api_key: str = Depends(get_api_key)):
    return {"message": "Access granted"}
```

### CORS Configuration

```python
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# Configure CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],  # React app
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Or allow all origins (dev only!)
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```


## 📁 File Operations

### File Uploads

```python
from fastapi import FastAPI, File, UploadFile
from typing import List
import shutil

app = FastAPI()

# Single file upload
@app.post("/upload")
async def upload_file(file: UploadFile = File(...)):
    """Upload a single file."""
    contents = await file.read()
    
    # Save file
    with open(f"uploads/{file.filename}", "wb") as f:
        f.write(contents)
    
    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "size": len(contents)
    }

# Multiple files
@app.post("/upload-multiple")
async def upload_multiple_files(files: List[UploadFile] = File(...)):
    """Upload multiple files."""
    return [
        {
            "filename": file.filename,
            "content_type": file.content_type
        }
        for file in files
    ]

# Stream large files
@app.post("/upload-large")
async def upload_large_file(file: UploadFile = File(...)):
    """Upload large file with streaming."""
    with open(f"uploads/{file.filename}", "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    
    return {"filename": file.filename}

# With form data
@app.post("/upload-with-data")
async def upload_with_data(
    file: UploadFile = File(...),
    description: str = Form(...),
    tags: List[str] = Form(...)
):
    return {
        "filename": file.filename,
        "description": description,
        "tags": tags
    }
```

### File Downloads

```python
from fastapi.responses import FileResponse, StreamingResponse
import os

@app.get("/download/{filename}")
async def download_file(filename: str):
    """Download a file."""
    file_path = f"files/{filename}"
    
    if not os.path.exists(file_path):
        raise HTTPException(status_code=404, detail="File not found")
    
    return FileResponse(
        path=file_path,
        filename=filename,
        media_type='application/octet-stream'
    )

# Stream large file
@app.get("/stream/{filename}")
async def stream_file(filename: str):
    """Stream a large file."""
    file_path = f"files/{filename}"
    
    def iterfile():
        with open(file_path, "rb") as f:
            yield from f
    
    return StreamingResponse(iterfile(), media_type="video/mp4")
```

### Static Files

```python
from fastapi.staticfiles import StaticFiles

# Mount static directory
app.mount("/static", StaticFiles(directory="static"), name="static")

# Now files in "static" folder are accessible:
# http://localhost:8000/static/image.jpg
# http://localhost:8000/static/css/styles.css
```


## ⏰ Background Tasks

```python
from fastapi import BackgroundTasks
import time

def write_log(message: str):
    """Simulate slow task."""
    time.sleep(5)  # Simulated delay
    with open("log.txt", "a") as f:
        f.write(f"{message}\n")

@app.post("/send-notification/{email}")
async def send_notification(
    email: str,
    background_tasks: BackgroundTasks
):
    """Send email in background."""
    background_tasks.add_task(write_log, f"Email sent to {email}")
    return {"message": "Notification sent in background"}

# Multiple background tasks
@app.post("/signup")
async def signup(
    email: str,
    background_tasks: BackgroundTasks
):
    """Sign up user with background tasks."""
    background_tasks.add_task(send_welcome_email, email)
    background_tasks.add_task(add_to_mailing_list, email)
    background_tasks.add_task(notify_admins, email)
    
    return {"message": "Signed up successfully"}
```


## 🔌 WebSockets

```python
from fastapi import WebSocket, WebSocketDisconnect
from typing import List

class ConnectionManager:
    def __init__(self):
        self.active_connections: List[WebSocket] = []
    
    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)
    
    def disconnect(self, websocket: WebSocket):
        self.active_connections.remove(websocket)
    
    async def broadcast(self, message: str):
        for connection in self.active_connections:
            await connection.send_text(message)

manager = ConnectionManager()

@app.websocket("/ws/{client_id}")
async def websocket_endpoint(websocket: WebSocket, client_id: int):
    await manager.connect(websocket)
    try:
        while True:
            data = await websocket.receive_text()
            await manager.broadcast(f"Client #{client_id}: {data}")
    except WebSocketDisconnect:
        manager.disconnect(websocket)
        await manager.broadcast(f"Client #{client_id} left the chat")
```


## 🧪 Testing

```bash
pip install pytest pytest-asyncio httpx
```

**test_main.py:**
```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_read_root():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello World"}

def test_create_user():
    response = client.post(
        "/users",
        json={"username": "alice", "email": "alice@example.com"}
    )
    assert response.status_code == 201
    assert response.json()["username"] == "alice"

# Async test
import pytest

@pytest.mark.asyncio
async def test_async_endpoint():
    async with AsyncClient(app=app, base_url="http://test") as ac:
        response = await ac.get("/async-route")
    assert response.status_code == 200
```


## 🚀 Production Deployment

### Docker

**Dockerfile:**
```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
    depends_on:
      - db
  
  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Environment Variables

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    app_name: str = "Awesome API"
    database_url: str
    secret_key: str
    debug: bool = False
    
    class Config:
        env_file = ".env"

settings = Settings()
```

**.env:**
```
DATABASE_URL=postgresql://user:pass@localhost/mydb
SECRET_KEY=your-secret-key
DEBUG=True
```


## 📚 Additional Resources

### Official Documentation
- **FastAPI Docs**: https://fastapi.tiangolo.com/
- **Pydantic**: https://docs.pydantic.dev/

### Best Practices Checklist

✅ **Development**
- Use type hints everywhere
- Enable auto-reload during development
- Write comprehensive tests
- Use dependency injection

✅ **Security**
- Never commit secrets
- Use environment variables
- Implement proper authentication
- Enable CORS carefully
- Validate all inputs

✅ **Performance**
- Use async for I/O operations
- Implement caching
- Add database indexes
- Use background tasks
- Enable compression

✅ **Production**
- Use Gunicorn with Uvicorn workers
- Set up logging and monitoring
- Implement health checks
- Use Docker containers
- Set up CI/CD pipeline


## 🎉 Conclusion

You've mastered FastAPI! You now know how to build production-grade APIs from scratch. 

---

<div align="center">
  
**Made with ❤️ for the Python Community by @RajeshTechForge**

</div>
