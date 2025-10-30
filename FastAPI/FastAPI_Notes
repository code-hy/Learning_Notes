
# A Detailed Tutorial on FastAPI

Welcome to this comprehensive tutorial on FastAPI! FastAPI is a modern, fast (high-performance) web framework for building APIs with Python 3.7+ based on standard Python type hints.

This tutorial will take you from the absolute basics to building a functional API, including data validation, error handling, and more.

## 🚀 Why Choose FastAPI?

* **Fast:** Very high performance, on par with **NodeJS** and **Go** (thanks to Starlette and Pydantic).
* **Fast to code:** Increases the speed to develop features by about 200% to 300%.
* **Fewer bugs:** Reduces about 40% of human-induced errors.
* **Intuitive:** Great editor support. Completion everywhere. Less time debugging.
* **Easy:** Designed to be easy to use and learn. Less time reading docs.
* **Short:** Minimizes code duplication.
* **Robust:** Get production-ready code. With automatic interactive documentation.
* **Standards-based:** Based on (and fully compatible with) the open standards for APIs: **OpenAPI** (formerly Swagger) and **JSON Schema**.

---

## 1. Setup and "Hello World"

Let's get our environment set up and run our first application.

### Installation

You'll need two main packages:
1.  `fastapi`: The framework itself.
2.  `uvicorn`: An "ASGI" server to run your application.

Install them both using `pip`:

```bash
pip install "fastapi[all]"
````

*(The `[all]` is optional but installs `uvicorn` and other useful dependencies.)*

### Create Your First App

Create a file named `main.py` and add the following code:

```python
from fastapi import FastAPI

# 1. Create a FastAPI instance
app = FastAPI()

# 2. Define a "path operation" (or "route")
@app.get("/")
async def root():
    # 3. Return the response
    return {"message": "Hello World"}
```

Let's break this down:

1.  We import `FastAPI` from the `fastapi` library.
2.  We create an `app` instance, which is the main point of interaction for your API.
3.  `@app.get("/")` is a **decorator** that tells FastAPI that the function directly below it is in charge of handling requests that go to:
      * The path `/`
      * Using a `get` HTTP method.
4.  `async def root():` defines an asynchronous function. You can also use a regular `def root():`, but `async` is recommended for I/O operations (like database or external API calls) so your server doesn't have to wait.
5.  The function returns a Python dictionary. FastAPI will automatically convert this dictionary into **JSON** format.

### Run the Application

Run your app from the terminal using `uvicorn`:

```bash
uvicorn main:app --reload
```

Let's break down this command:

  * `main`: The name of your Python file (`main.py`).
  * `app`: The `FastAPI` instance you created inside `main.py` (`app = FastAPI()`).
  * `--reload`: This tells `uvicorn` to automatically restart the server whenever you save changes to your code. This is fantastic for development.

You should see output similar to this:

```
INFO:     Uvicorn running on [http://127.0.0.1:8000](http://127.0.0.1:8000) (Press CTRL+C to quit)
INFO:     Started reloader process [12345]
INFO:     Started server process [12347]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

### Check Your "Hello World"

Open your browser and go to **https://www.google.com/url?sa=E\&source=gmail\&q=http://127.0.0.1:8000**. You will see the JSON response:

```json
{
  "message": "Hello World"
}
```

### 🎁 The Automatic Docs (A Key Feature\!)

FastAPI automatically generates interactive API documentation for you. This is one of its most powerful features.

1.  Go to **https://www.google.com/search?q=http://127.0.0.1:8000/docs** in your browser.
2.  You will see the "Swagger UI," an interactive page where you can see all your endpoints and even test them live.

You also get an alternative documentation page:

1.  Go to **https://www.google.com/search?q=http://127.0.0.1:8000/redoc** in your browser.
2.  You will see the "ReDoc" documentation, which provides a cleaner, more static view of your API.

-----

## 2\. Path and Query Parameters

Parameters are how you pass dynamic information to your endpoints. FastAPI uses Python type hints to define and validate them.

### Path Parameters

You can declare path "variables" using the same f-string-like syntax in the decorator.

Let's update `main.py`:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}

# Add this new endpoint:
@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

Now, go to **https://www.google.com/search?q=http://127.0.0.1:8000/items/5**. You'll see:

```json
{
  "item_id": 5
}
```

Notice what happened:

  * The value `5` was passed from the URL to the function argument `item_id`.
  * Because we used the type hint `item_id: int`, FastAPI automatically **validates** and **converts** the data.
  * Try going to **https://www.google.com/search?q=http://127.0.0.1:8000/items/foo**. You'll get a nice, clear JSON error message because `"foo"` is not a valid `int`.

### Query Parameters

Query parameters are key-value pairs that come *after* the `?` in a URL.

Example: `http://127.0.0.1:8000/items/?skip=0&limit=10`

To define query parameters, simply declare function arguments that are *not* part of the path.

Let's modify `main.py` again:

```python
from fastapi import FastAPI

app = FastAPI()

# A fake "database" of items
fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


@app.get("/items/")
async def read_items(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    # This is just an example; in a real app, you'd fetch from a database
    return {"item_id": item_id, "name": f"Item {item_id}"}
```

Now, try these URLs:

  * **https://www.google.com/search?q=http://127.0.0.1:8000/items/**
      * *Result:* `[{"item_name":"Foo"},{"item_name":"Bar"},{"item_name":"Baz"}]`
      * *Why:* It uses the default values `skip=0` and `limit=10`.
  * **https://www.google.com/search?q=http://127.0.0.1:8000/items/%3Fskip%3D1%26limit%3D2**
      * *Result:* `[{"item_name":"Bar"},{"item_name":"Baz"}]`
      * *Why:* It uses the values you provided.

#### Optional and Required Parameters

You can make query parameters optional by giving them a default value (like we did with `skip: int = 0`). To make them truly optional (i.e., they can be `None`), use `| None = None` (or `Optional[str] = None`).

```python
from fastapi import FastAPI
from typing import Optional # Or use | None for Python 3.10+

app = FastAPI()

@app.get("/users/{user_id}/items/{item_id}")
async def read_user_item(
    user_id: int, 
    item_id: str, 
    q: str | None = None, # Optional query parameter
    short: bool = False   # Optional query param with a default
):
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```

-----

## 3\. Request Body (Pydantic Models)

When you need to send data from a client to your API (e.g., with a `POST`, `PUT`, or `PATCH` request), you use a **Request Body**.

FastAPI uses the **Pydantic** library to define the *shape* of your data using Python classes. This gives you incredible power:

  * **Data Validation:** Ensures incoming data matches the required types.
  * **Data Conversion:** Converts JSON to Python objects.
  * **Editor Support:** You get autocompletion for your data models.
  * **Documentation:** Your data models are automatically included in the `/docs`.

### Define Your Pydantic Model

Let's update `main.py` to include a Pydantic model.

```python
from fastapi import FastAPI
from pydantic import BaseModel # Import BaseModel

app = FastAPI()

# 1. Define your data model
class Item(BaseModel):
    name: str
    description: str | None = None  # An optional description
    price: float
    tax: float | None = None      # An optional tax
```

Here, `Item` is a Pydantic model that defines the *shape* of an item. It must have a `name` (string) and a `price` (float), but `description` and `tax` are optional.

### Use the Model in Your Endpoint

Now, let's create a `POST` endpoint that *receives* an `Item` object.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

# 2. Use the model as a type hint
@app.post("/items/")
async def create_item(item: Item):
    # 3. You can access the data as object attributes
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    
    return item_dict
```

**What's happening here?**

When you declare `item: Item` as a function argument, FastAPI will:

1.  Read the body of the `POST` request as JSON.
2.  Validate that the JSON has the required fields (`name` and `price`).
3.  Check that the types are correct (e.g., `price` is a `float`).
4.  If the data is invalid, it automatically returns a `422 Unprocessable Entity` error.
5.  If the data is valid, it creates an instance of your `Item` class and passes it to your function as the `item` argument.

You can now test this in your **`/docs`** (https://www.google.com/search?q=http://127.0.0.1:8000/docs).

1.  Find the `POST /items/` endpoint and expand it.
2.  Click "Try it out".
3.  Edit the "Request body" JSON and click "Execute".
4.  You'll see your API's response\!

-----

## 4\. Error Handling

Sometimes, you need to stop a request and return an error to the client (e.g., "Item not found"). You can do this by raising an `HTTPException`.

Let's add `HTTPException` to our `main.py`.

```python
from fastapi import FastAPI, HTTPException # Import HTTPException
from pydantic import BaseModel

app = FastAPI()

# --- Model Definition ---
class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

# --- Fake Database ---
fake_db = {
    1: {"name": "Foo", "price": 50.2},
    2: {"name": "Bar", "price": 62.0, "tax": 5.5},
    3: {"name": "Baz", "price": 19.9, "description": "A very nice baz"},
}

# --- Endpoints ---

@app.get("/")
async def root():
    return {"message": "Welcome to the API"}

@app.get("/db-items/{item_id}")
async def read_db_item(item_id: int):
    if item_id not in fake_db:
        # Raise an exception if the item is not found
        raise HTTPException(status_code=404, detail="Item not found")
    
    # If found, return the item
    return fake_db[item_id]

@app.post("/items/")
async def create_item(item: Item):
    # In a real app, you'd save this to the database
    print("Item created:", item.dict())
    return item
```

Now, if you go to **https://www.google.com/search?q=http://127.0.0.1:8000/db-items/1**, you'll get the item.
But if you go to **https://www.google.com/search?q=http://127.0.0.1:8000/db-items/99**, FastAPI will stop execution and return this JSON response with a `404` status code:

```json
{
  "detail": "Item not found"
}
```

-----

## 5\. Conclusion and Next Steps

You have just learned the core fundamentals of FastAPI\!

You now know how to:

  * **Create** a basic FastAPI application and **run** it with `uvicorn`.
  * Use the automatic interactive **documentation** at `/docs`.
  * Define endpoints with different **HTTP methods** (`GET`, `POST`).
  * Receive data using **Path Parameters**, **Query Parameters**, and **Request Bodies**.
  * Use **Pydantic** models to get powerful data **validation**, conversion, and documentation.
  * Handle errors gracefully by raising `HTTPException`.

From here, you can explore more advanced topics:

  * **Dependencies:** Create reusable logic (e.g., for authentication or database connections) that can be "injected" into your endpoints.
  * **Security:** Implement authentication and authorization (e.g., OAuth2 with JWT tokens).
  * **Testing:** Write unit and integration tests for your API using `TestClient`.
  * **Async/Await:** Connect to databases and other services asynchronously.

The official FastAPI documentation is one of the best resources available and covers all these topics in great detail.

```
```
