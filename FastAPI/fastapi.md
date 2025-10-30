



### File: Fastapi.md

```markdown
# A Detailed Tutorial on FastAPI

Welcome to this comprehensive tutorial on FastAPI, a modern, high-performance web framework for building APIs with Python. This guide will take you from the basics to creating a fully functional API with data validation.

## Table of Contents
1.  [What is FastAPI?](#what-is-fastapi)
2.  [Prerequisites](#prerequisites)
3.  [Installation](#installation)
4.  [Your First API: "Hello, World"](#your-first-api-hello-world)
5.  [Running the Application](#running-the-application)
6.  [Interactive API Documentation](#interactive-api-documentation)
7.  [Path Parameters](#path-parameters)
8.  [Query Parameters](#query-parameters)
9.  [Request Body with Pydantic Models](#request-body-with-pydantic-models)
10. [Putting It All Together](#putting-it-all-together)
11. [Next Steps](#next-steps)

---

## What is FastAPI?

FastAPI is a web framework for building APIs. Its key features are:

*   **Fast:** Very high performance, on par with NodeJS and Go (thanks to Starlette and Pydantic).
*   **Fast to Code:** Increases the speed of developing features by about 200% to 300%.
*   **Fewer Bugs:** Reduces about 40% of human-induced errors.
*   **Intuitive:** Great editor support, with autocomplete everywhere. Less time debugging.
*   **Easy:** Designed to be easy to use and learn. Less time reading docs.
*   **Standards-Based:** Based on (and fully compatible with) the open standards for APIs: **OpenAPI** (formerly known as Swagger) and **JSON Schema**.

---

## Prerequisites

Before you start, make sure you have the following installed:
*   **Python 3.7+**
*   A code editor (like VS Code)
*   A terminal or command prompt

---

## Installation

You'll need two main packages: `fastapi` for the framework itself and `uvicorn` to be the server that runs your code.

```bash
pip install "fastapi[all]"
```
> **Note:** Installing `fastapi[all]` includes `uvicorn` and other useful dependencies. For production, you might want to install `fastapi` and `uvicorn` separately: `pip install fastapi "uvicorn[standard]"`.

---

## Your First API: "Hello, World"

Let's create a minimal API. Create a file named `main.py`:

```python
# main.py

from fastapi import FastAPI

# Create an instance of the FastAPI class
app = FastAPI()

# Define a path operation decorator
@app.get("/")
# Define the path operation function
async def read_root():
    return {"Hello": "World"}
```

**Explanation:**
1.  `from fastapi import FastAPI`: Imports the main `FastAPI` class.
2.  `app = FastAPI()`: Creates an instance of your application. This will be the central point of your API.
3.  `@app.get("/")`: This is a "decorator". It tells FastAPI that the function right below it is in charge of handling requests that go to:
    *   The path `/`.
    *   Using the HTTP `GET` operation.
4.  `async def read_root():`: An `async` function. FastAPI can handle `async` functions natively, which is great for performance.
5.  `return {"Hello": "World"}`: FastAPI will take this dictionary, convert it to JSON, and send it back to the client.

---

## Running the Application

Open your terminal in the same directory where you saved `main.py` and run the following command:

```bash
uvicorn main:app --reload
```

**Explanation of the command:**
*   `uvicorn`: The web server.
*   `main`: The file `main.py`.
*   `app`: The object `app = FastAPI()` created inside `main.py`.
*   `--reload`: Makes the server restart after code changes. Only use for development.

You will see output like this:

```
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [12345]
INFO:     Started server process [12347]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

Now, open your browser and go to `http://127.0.0.1:8000`. You will see the JSON response: `{"Hello":"World"}`.

---

## Interactive API Documentation

This is one of the most powerful features of FastAPI. Go to `http://127.0.0.1:8000/docs` in your browser.

You will see an automatic, interactive API documentation (provided by Swagger UI):


You can click on the endpoint, try it out, and see the results directly from your browser.

There's also an alternative documentation at `http://127.0.0.1:8000/redoc`.

---

## Path Parameters

You can declare path "parameters" or "variables" with the same syntax used by Python format strings.

```python
# main.py

from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

The value of `item_id` will be passed to your function as the argument `item_id`.

### Type Hints for Validation

By declaring the type of `item_id` as `int`, FastAPI provides automatic validation and documentation.

*   If you try to access `/items/foo`, you will see a clear HTTP error, because `foo` is not an integer.
*   The documentation at `/docs` will show that the parameter is an integer.

---

## Query Parameters

Query parameters are the key-value pairs that come after the `?` in a URL, separated by `&`.

Declare them in the function as normal function arguments.

```python
# main.py

from fastapi import FastAPI
from typing import Union

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```

**Explanation:**
*   `skip: int = 0`: `skip` is an optional query parameter of type `int` that defaults to `0`.
*   `limit: int = 10`: `limit` is an optional query parameter of type `int` that defaults to `10`.

You can now access URLs like:
*   `http://127.0.0.1:8000/items/` (Defaults: `skip=0`, `limit=10`)
*   `http://127.0.0.1:8000/items/?skip=2` (Defaults: `limit=10`)
*   `http://127.0.0.1:8000/items/?skip=2&limit=5`

---

## Request Body with Pydantic Models

When you need to send data from a client (e.g., in a `POST` request), you declare the data model using Pydantic.

First, import `BaseModel` from `pydantic`.

```python
# main.py

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Define a Pydantic model
class Item(BaseModel):
    name: str
    description: Union[str, None] = None
    price: float
    tax: Union[float, None] = None

@app.post("/items/")
async def create_item(item: Item):
    return item
```

**Explanation:**
1.  **`class Item(BaseModel):`**: We create a data model that inherits from Pydantic's `BaseModel`.
2.  **Attributes**: We declare the fields with their types. `Union[str, None]` (or `str | None` in Python 3.10+) means the field is optional.
3.  **`@app.post("/items/")`**: We use a `POST` operation, which is typically used to create new data.
4.  **`async def create_item(item: Item):`**: We declare the function parameter `item` with the type `Item`.

With this, FastAPI will:
*   Read the body of the request as JSON.
*   Validate the data against the `Item` model. If it's invalid, it will return a clear error.
*   Convert the data into the defined Python types (e.g., `price` will be a `float`).
*   Give you the validated data in the `item` parameter.
*   Generate automatic documentation for the request body.

---

## Putting It All Together

Here is a more complete example combining path parameters, query parameters, and a request body.

```python
# main.py

from fastapi import FastAPI
from pydantic import BaseModel
from typing import Union

app = FastAPI()

class Item(BaseModel):
    name: str
    description: Union[str, None] = None
    price: float
    tax: Union[float, None] = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item, q: Union[str, None] = None):
    result = {"item_id": item_id, **item.model_dump()}
    if q:
        result.update({"q": q})
    return result
```

*   `item_id`: A path parameter (integer).
*   `item`: A request body (Pydantic model).
*   `q`: An optional query parameter (string).

---

## Next Steps

You've covered the fundamentals of FastAPI! From here, you can explore more advanced topics:
*   **Dependency Injection**: A powerful system to share logic, databases, user authentication, etc.
*   **Security**: Implementing authentication and authorization (e.g., OAuth2 with JWT tokens).
*   **Middleware**: Code that runs before and after each request.
*   **Background Tasks**: Running long-running processes after returning a response.
*   **Databases**: Connecting to SQL (with SQLAlchemy) or NoSQL databases.
*   **Deployment**: Using Docker and Uvicorn to deploy your application.

The official [FastAPI documentation](https://fastapi.tiangolo.com/) is an excellent resource for diving deeper.
```
