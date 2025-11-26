# Exercise: Getting Started with FastAPI

## Exercise Overview

In this exercise, you'll use AI to quickly get up to speed with FastAPI, a modern Python web framework for building APIs. You'll go from zero knowledge to creating a working API with proper structure.

---

## Part 1: Understanding FastAPI Fundamentals

### Task: Use AI to learn the basics of FastAPI

### Prompts to Try

1. **"What is FastAPI and how does it compare to Flask and Django?"**

2. **"Explain the core concepts and terminology used in FastAPI."**

3. **"What are the key advantages of FastAPI over other Python web frameworks?"**

4. **"Create a glossary of essential FastAPI terms and concepts."**

### Goal

Take notes on FastAPI's purpose, design philosophy, and key features.

### Example AI Response Summary

**What is FastAPI?**
- Modern, fast web framework for building APIs with Python
- Based on standard Python type hints
- Uses Pydantic for data validation
- Automatic interactive API documentation
- Built on Starlette and Pydantic

**Key Advantages:**
- **Performance**: One of the fastest Python frameworks
- **Type Safety**: Leverages Python type hints
- **Automatic Documentation**: OpenAPI/Swagger docs generated automatically
- **Data Validation**: Automatic request/response validation
- **Modern Python**: Uses async/await natively

**Core Concepts:**
- **FastAPI Instance**: The main application object
- **Path Operations**: Functions that handle HTTP requests
- **Path Parameters**: Variables in the URL path
- **Query Parameters**: Optional parameters after `?` in URL
- **Request Body**: Data sent in POST/PUT requests
- **Response Models**: Pydantic models that define response structure
- **Dependencies**: Reusable components injected into routes

---

## Part 2: Creating Your First API

### Task: Use AI to help you build a simple "Hello World" API with FastAPI

### Prompts to Try

1. **"Generate a basic 'Hello World' example for FastAPI with comments explaining each part."**

2. **"How do I structure a FastAPI project following best practices?"**

3. **"Show me how to run and test a FastAPI application locally."**

### Implementation

Create a file named `main.py`:

```python
# main.py
from fastapi import FastAPI

# Create a FastAPI instance - this is the core of your application
app = FastAPI(
    title="My First FastAPI App",
    description="A simple API built with FastAPI",
    version="0.1.0"
)

# Define a route using a decorator
# The @app.get("/") decorator tells FastAPI that the function below handles GET requests to the root path "/"
@app.get("/")
async def root():
    """
    Root endpoint that returns a simple greeting message
    Returns a JSON object with a welcome message
    """
    return {"message": "Hello World from FastAPI!"}

# Define another route with a path parameter
# Path parameters are variables in the path that are captured and passed to the function
@app.get("/items/{item_id}")
async def read_item(item_id: int):
    """
    Endpoint that returns information about a specific item

    Args:
        item_id (int): The ID of the item to retrieve

    Returns:
        JSON object with the item ID and a message
    """
    return {"item_id": item_id, "message": f"You requested item {item_id}"}

# Define a route with query parameters
# Query parameters are optional and are specified after the ? in the URL
@app.get("/search/")
async def search_items(q: str = None, skip: int = 0, limit: int = 10):
    """
    Endpoint that searches for items based on query parameters

    Args:
        q (str, optional): Search query
        skip (int, optional): Number of items to skip
        limit (int, optional): Maximum number of items to return

    Returns:
        JSON object with the search parameters and results
    """
    return {
        "query": q,
        "skip": skip,
        "limit": limit,
        "message": f"Searching for '{q}' (skipping {skip}, limiting to {limit})"
    }

# To run this app, use the command:
# uvicorn main:app --reload
#
# Then you can access:
# - http://127.0.0.1:8000/ for the root endpoint
# - http://127.0.0.1:8000/items/42 to get information about item 42
# - http://127.0.0.1:8000/search?q=test to search for "test"
# - http://127.0.0.1:8000/docs for the automatic interactive API documentation
```

### Running the Application

1. **Install dependencies:**
   ```bash
   pip install fastapi uvicorn
   ```

2. **Run the server:**
   ```bash
   uvicorn main:app --reload
   ```

3. **Access the API:**
   - API: http://127.0.0.1:8000/
   - Interactive Docs: http://127.0.0.1:8000/docs
   - Alternative Docs: http://127.0.0.1:8000/redoc

### Testing the Endpoints

**Using the Interactive Docs:**
1. Visit http://127.0.0.1:8000/docs
2. Click on an endpoint to expand it
3. Click "Try it out"
4. Enter parameters and click "Execute"
5. See the response

**Using curl:**
```bash
# Root endpoint
curl http://127.0.0.1:8000/

# Path parameter
curl http://127.0.0.1:8000/items/42

# Query parameters
curl "http://127.0.0.1:8000/search/?q=test&skip=0&limit=5"
```

---

## Part 3: Enhancing Your API

### Task: Use AI to help you add more advanced features to your basic API

### Prompts to Try

1. **"How do I add request body validation with Pydantic models in FastAPI?"**

2. **"Show me how to implement proper error handling in FastAPI."**

3. **"How can I organize my FastAPI project into multiple files and modules?"**

### Enhanced Implementation

**Project Structure:**
```
my_fastapi_app/
├── app/
│   ├── __init__.py
│   ├── main.py          # FastAPI application creation and configuration
│   ├── models/          # Pydantic models for request/response
│   │   ├── __init__.py
│   │   └── item.py      # Item data models
│   ├── routes/          # API route handlers
│   │   ├── __init__.py
│   │   └── items.py     # Item-related endpoints
│   └── utils/           # Utility functions and helpers
│       ├── __init__.py
│       └── exceptions.py # Custom exception handlers
└── requirements.txt     # Project dependencies
```

**Key Files:**

**app/models/item.py:**
```python
from pydantic import BaseModel, Field
from typing import Optional, List

class ItemBase(BaseModel):
    """Base model for item data"""
    name: str = Field(..., min_length=1, max_length=100, description="The name of the item")
    description: Optional[str] = Field(None, max_length=1000, description="Optional description")
    price: float = Field(..., gt=0, description="Price must be greater than zero")
    tags: List[str] = Field(default=[], description="List of tags for the item")

class ItemCreate(ItemBase):
    """Model for creating a new item"""
    pass

class ItemResponse(ItemBase):
    """Model for item responses that includes the ID"""
    id: int

    class Config:
        schema_extra = {
            "example": {
                "id": 1,
                "name": "Laptop",
                "description": "Powerful development machine",
                "price": 1299.99,
                "tags": ["electronics", "computers"]
            }
        }
```

**app/utils/exceptions.py:**
```python
from fastapi import FastAPI, Request, status
from fastapi.responses import JSONResponse
from fastapi.exceptions import RequestValidationError

class ItemNotFoundError(Exception):
    """Raised when an item is not found"""
    def __init__(self, item_id: int):
        self.item_id = item_id
        self.message = f"Item with ID {item_id} not found"
        super().__init__(self.message)

def add_exception_handlers(app: FastAPI) -> None:
    """Add custom exception handlers to the FastAPI application"""

    @app.exception_handler(ItemNotFoundError)
    async def item_not_found_handler(request: Request, exc: ItemNotFoundError):
        return JSONResponse(
            status_code=status.HTTP_404_NOT_FOUND,
            content={"detail": exc.message}
        )

    @app.exception_handler(RequestValidationError)
    async def validation_exception_handler(request: Request, exc: RequestValidationError):
        return JSONResponse(
            status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
            content={
                "detail": "Validation error in request data",
                "errors": exc.errors()
            }
        )
```

**app/routes/items.py:**
```python
from fastapi import APIRouter, Path, Query, HTTPException, status
from typing import List, Optional

from ..models.item import ItemCreate, ItemResponse
from ..utils.exceptions import ItemNotFoundError

router = APIRouter(prefix="/items", tags=["items"])

# Mock database (in a real app, this would be a database)
fake_items_db = {}
item_counter = 0

@router.post("/", response_model=ItemResponse, status_code=status.HTTP_201_CREATED)
async def create_item(item: ItemCreate):
    """Create a new item"""
    global item_counter
    item_counter += 1

    # Create new item with ID
    item_dict = item.dict()
    new_item = {**item_dict, "id": item_counter}

    # Store in our fake database
    fake_items_db[item_counter] = new_item

    return new_item

@router.get("/{item_id}", response_model=ItemResponse)
async def read_item(
    item_id: int = Path(..., gt=0, description="The ID of the item to retrieve")
):
    """Get a specific item by ID"""
    if item_id not in fake_items_db:
        raise ItemNotFoundError(item_id)

    return fake_items_db[item_id]

@router.get("/", response_model=List[ItemResponse])
async def list_items(
    skip: int = Query(0, ge=0, description="Number of items to skip"),
    limit: int = Query(10, ge=1, le=100, description="Maximum number of items to return"),
    tag: Optional[str] = Query(None, description="Filter items by tag")
):
    """List items with optional filtering and pagination"""
    items = list(fake_items_db.values())

    # Filter by tag if provided
    if tag:
        items = [item for item in items if tag in item.get("tags", [])]

    # Apply pagination
    return items[skip:skip+limit]
```

**app/main.py:**
```python
from fastapi import FastAPI
from .routes import items
from .utils.exceptions import add_exception_handlers

# Create FastAPI application
app = FastAPI(
    title="Enhanced FastAPI Example",
    description="A more structured FastAPI application with proper models and error handling",
    version="0.2.0"
)

# Add routes
app.include_router(items.router)

# Configure exception handlers
add_exception_handlers(app)

# Root endpoint
@app.get("/", tags=["root"])
async def root():
    """API root endpoint"""
    return {
        "message": "Welcome to the enhanced FastAPI example",
        "docs": "/docs",
        "endpoints": {
            "items": "/items"
        }
    }

# To run: uvicorn app.main:app --reload
```

---

## Part 4: Exercise Challenge

### Task: Create a To-Do List API

Using what you've learned and with AI assistance, create a simple FastAPI application for managing a to-do list with the following features:

1. **Create a new to-do item** with title, description, and due date
2. **List all to-do items** with optional filtering by status (completed/pending)
3. **Mark a to-do item as completed**
4. **Delete a to-do item**

### Suggested Implementation Steps

1. **Define Pydantic models** for the to-do items
   - `TodoCreate`: For creating new todos
   - `TodoUpdate`: For updating todos
   - `TodoResponse`: For API responses

2. **Create endpoints** for CRUD operations
   - `POST /todos/`: Create a new todo
   - `GET /todos/`: List all todos (with filtering)
   - `GET /todos/{todo_id}`: Get a specific todo
   - `PUT /todos/{todo_id}`: Update a todo
   - `PATCH /todos/{todo_id}/complete`: Mark as completed
   - `DELETE /todos/{todo_id}`: Delete a todo

3. **Implement proper validation** and error handling
   - Validate required fields
   - Handle invalid IDs
   - Return appropriate error messages

4. **Add filtering capabilities** for the list endpoint
   - Filter by status (completed/pending)
   - Filter by due date range
   - Pagination support

5. **Document your API** using FastAPI's automatic documentation features
   - Add descriptions to endpoints
   - Include example requests/responses
   - Use response models

### Tips for Using AI Effectively

1. **Start broad, then narrow:**
   - "How do I create a CRUD API in FastAPI?"
   - "Show me how to implement filtering in FastAPI endpoints"
   - "How do I handle date validation in Pydantic models?"

2. **When you encounter errors:**
   - Share the complete error message with AI
   - Include relevant code snippets
   - Ask for troubleshooting help

3. **For unfamiliar concepts:**
   - "What is Pydantic and how does FastAPI use it?"
   - "Explain FastAPI's dependency injection system"
   - "How does FastAPI's automatic documentation work?"

4. **Request examples:**
   - "Show me a complete example of a FastAPI endpoint with validation"
   - "Give me an example of error handling in FastAPI"

### Example Implementation Structure

```python
# models/todo.py
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime
from enum import Enum

class TodoStatus(str, Enum):
    PENDING = "pending"
    COMPLETED = "completed"

class TodoBase(BaseModel):
    title: str = Field(..., min_length=1, max_length=200)
    description: Optional[str] = None
    due_date: Optional[datetime] = None

class TodoCreate(TodoBase):
    pass

class TodoUpdate(BaseModel):
    title: Optional[str] = None
    description: Optional[str] = None
    due_date: Optional[datetime] = None
    status: Optional[TodoStatus] = None

class TodoResponse(TodoBase):
    id: int
    status: TodoStatus
    created_at: datetime
    
    class Config:
        orm_mode = True
```

---

## Reflection Questions

After completing this exercise, consider:

1. **What surprised you about FastAPI compared to other frameworks you've used?**
2. **How did AI help you learn FastAPI more quickly than reading documentation alone?**
3. **What concepts do you still need to explore further?**
4. **How would you structure a larger FastAPI application?**

---

## Next Steps

1. Explore FastAPI's advanced features:
   - Dependency injection
   - Background tasks
   - WebSockets
   - Database integration (SQLAlchemy, Tortoise ORM)

2. Build a more complex project:
   - Add authentication
   - Integrate with a database
   - Add testing
   - Deploy to production

3. Continue learning:
   - Read FastAPI documentation
   - Join FastAPI community
   - Contribute to open-source FastAPI projects

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

