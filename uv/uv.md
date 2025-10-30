
### File: uv.md

```markdown
# A Detailed Tutorial on `uv`

This tutorial introduces `uv`, an extremely fast Python package installer and resolver, written in Rust. It's designed to be a modern replacement for `pip` and `venv`, offering a significant speed boost and a more integrated workflow.

## Table of Contents
1.  [What is `uv`? The "Why"](#what-is-uv-the-why)
2.  [Installation](#installation)
3.  [Core Concepts and Basic Usage](#core-concepts-and-basic-usage)
4.  [A Complete Workflow Example](#a-complete-workflow-example)
5.  [Comparison: `uv` vs. `pip` + `venv`](#comparison-uv-vs-pip--venv)
6.  [Next Steps](#next-steps)

---

## What is `uv`? The "Why"

If you've ever waited for `pip install` to resolve dependencies, you'll appreciate `uv`. It is a drop-in replacement for `pip` and `pip-tools` that is **10-100x faster**.

Key benefits:
*   **Blazing Fast**: Dependency resolution and installation are incredibly quick.
*   **All-in-One Tool**: Combines the functionality of `pip`, `virtualenv`, and `pip-tools` into a single, unified tool.
*   **`pip` Compatible**: It works with your existing `requirements.txt` files and the Python Package Index (PyPI).
*   **Modern Project Management**: Uses `pyproject.toml` as the single source of truth for your project's metadata and dependencies.

---

## Installation

The recommended way to install `uv` is with the official installer script.

**On macOS and Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**On Windows (PowerShell):**
```bash
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

After installation, you may need to restart your terminal. Verify the installation with:
```bash
uv --version
```

---

## Core Concepts and Basic Usage

`uv` streamlines the entire Python project lifecycle.

### 1. Creating a Project

Instead of manually creating a virtual environment and a `pyproject.toml`, `uv` does it for you.

```bash
# Create a new project in a directory called "my-awesome-app"
uv init my-awesome-app

# Navigate into the new directory
cd my-awesome-app
```

This command creates:
*   `my-awesome-app/`: The project directory.
*   `pyproject.toml`: A file with project metadata (name, version) and a `[project]` section.
*   `.venv/`: A virtual environment is created automatically inside the project directory.
*   `src/my_awesome_app/`: A source directory for your code.
*   `README.md`: A basic readme file.

### 2. Managing Dependencies

This is where `uv` truly shines. You don't need to activate the virtual environment. `uv` commands automatically find and use the `.venv` in your project.

**Adding a Production Dependency:**
```bash
uv add fastapi
```
This command will:
1.  Resolve the latest compatible version of `fastapi` and its dependencies.
2.  Download and install them into the `.venv`.
3.  Add `fastapi` to your `pyproject.toml` under `[project.dependencies]`.

**Adding a Development Dependency:**
For tools like `pytest` or `black` that are only needed for development:
```bash
uv add --dev pytest
```
This adds the package to a separate `[project.optional-dependencies.dev]` section in `pyproject.toml`.

### 3. Running Commands

You no longer need to `source .venv/bin/activate`. Use `uv run` to execute commands within the project's virtual environment.

```bash
# Run a Python script
uv run python main.py

# Run a server (like Uvicorn for FastAPI)
uv run uvicorn main:app --reload

# Run a linter
uv run ruff check .
```

`uv run` ensures the command uses the Python interpreter and packages from the project's `.venv`.

---

## A Complete Workflow Example

Let's build a simple FastAPI application from scratch using `uv`.

**Step 1: Initialize the project**
```bash
uv init fastapi-demo && cd fastapi-demo
```

**Step 2: Add FastAPI as a dependency**
```bash
uv add "fastapi[standard]"
```
> **Note:** We use quotes to ensure the shell passes `[standard]` as part of the package name. This installs FastAPI with its recommended extra dependencies.

**Step 3: Create the application file**
Create a file named `main.py` in the root of your project (`fastapi-demo/`).

`main.py`:
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello from a project managed by uv!"}
```

**Step 4: Run the application**
```bash
uv run uvicorn main:app --reload
```
Open your browser to `http://127.0.0.1:8000`. You'll see the JSON response.

**Step 5: Add a test dependency and write a test**
```bash
uv add --dev httpx
```
`httpx` is a modern HTTP client that we'll use for testing.

Create a `test_main.py` file:
`test_main.py`:
```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_read_root():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello from a project managed by uv!"}
```

**Step 6: Run the tests**
```bash
uv run pytest
```
`uv` will find `pytest` (which was installed as a dev dependency with FastAPI) and run your tests.

---

## Comparison: `uv` vs. `pip` + `venv`

Here's a side-by-side comparison of common tasks.

| Task                        | Traditional Way (`pip` + `venv`)                                 | Modern Way (`uv`)                                         |
| --------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------- |
| **Create Project**          | `python -m venv .venv`<br>`source .venv/bin/activate`<br>`pip install fastapi` | `uv init my-project`<br>`cd my-project`<br>`uv add fastapi` |
| **Activate Environment**    | `source .venv/bin/activate` (or `.venv\Scripts\activate`)         | **Not needed!** `uv` commands are environment-aware.       |
| **Install Package**         | `pip install requests`                                            | `uv add requests`                                         |
| **Install Dev Package**     | `pip install pytest`                                              | `uv add --dev pytest`                                     |
| **Run Script**              | `python my_script.py` (after activation)                          | `uv run python my_script.py`                              |
| **Install from `requirements.txt`** | `pip install -r requirements.txt`                                | `uv pip install -r requirements.txt` (for compatibility)  |
| **Dependency Source**       | `requirements.txt`, `setup.py`, `pyproject.toml` (mixed)          | `pyproject.toml` (single source of truth)                 |

---

## Next Steps

You've mastered the basics of `uv`! Here's what to explore next:
*   **`uv pip`**: Use `uv` as a direct, much faster replacement for `pip` in any environment, even without a `pyproject.toml`.
*   **`uv script`**: Run single-file Python scripts with inline dependency definitions.
*   **`uv python`**: Manage multiple Python installations directly with `uv`.
*   **Global Configuration**: Learn how to configure `uv`'s behavior with config files or environment variables.

For more information, check out the official [`uv` documentation](https://docs.astral.sh/uv/).
```
