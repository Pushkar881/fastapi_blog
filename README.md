### ** LECTURE 1 NOTES**


This video serves as the first part of a series on building a **full-featured web application** and **REST API** using **FastAPI**. Throughout the series, you will learn to build an application featuring a JSON REST API for programmatic access and HTML pages for users, utilizing tools like **SQLAlchemy** for database management, **Pydantic** for data validation, and **JWT tokens** for secure authentication.

In this specific introductory video, the tutorial covers setting up your environment, creating basic routes, and understanding the automatic documentation features.

### **1. Initial Setup and Installation**
To begin, you create a project directory (e.g., `fastapi_blog`). The tutorial uses **uv**, a modern and fast Python package manager, though standard **pip** works as well. 

To install FastAPI with all recommended extras (including the **Uvicorn** ASGI server and the **FastAPI CLI**), use the following commands:
*   **Using pip:** `pip install "fastapi[standard]"`
*   **Using uv:** `uv add fastapi[standard]`

### **2. Creating a Basic Application**
In a file named `main.py`, you initialize the application by importing the FastAPI class and creating an instance:
```python
from fastapi import FastAPI

app = FastAPI()
```
This `app` object is used to define your **routes** via decorators, similar to the Flask framework.

### **3. Building API Routes**
Routes are created using decorators like `@app.get()`.
*   **Root Route:** You can define a home route that returns a simple dictionary. FastAPI **automatically converts dictionaries to JSON** for the response.
*   **Synchronous vs. Asynchronous:** While FastAPI supports `async def`, the tutorial begins with standard `def` synchronous functions for simplicity.
*   **Data Handling:** To simulate real data, the video introduces a list of dictionaries representing blog posts. A new route, `/api/posts`, is created to return this list, which FastAPI serves as a **JSON array**.

### **4. Running the Application**
To run the server, use the FastAPI CLI in your terminal:
*   **Command:** `fastapi dev main.py` (or `uv run fastapi dev main.py` if using uv).
*   **Dev Mode:** The `dev` command is crucial during development because it includes **auto-reload**, which restarts the server every time you save a code change.
*   **Production:** In a production environment, you would use `fastapi run` for optimized performance.

### **5. Automatic API Documentation**
One of FastAPI's standout features is its **automatic documentation**, which requires no extra configuration.
*   **Swagger UI:** Accessible at `/docs`, this interactive interface allows you to test your API endpoints directly in the browser.
*   **ReDoc:** Accessible at `/redoc`, this provides an alternative, more modern-looking layout for the same documentation.
*   **Testing:** The Swagger UI even provides **curl commands** that you can copy and paste into your terminal to test endpoints programmatically.

### **6. Returning HTML Responses**
While the backend focuses on JSON, you can also serve HTML for human users.
*   **HTMLResponse:** You must import `HTMLResponse` from `fastapi.responses` and specify `response_class=HTMLResponse` within your route decorator.
*   **Stacking Decorators:** You can stack multiple decorators on a single function if you want the same content to be available at different URLs (e.g., both `/` and `/posts`).
*   **Hiding Routes from Docs:** Since HTML routes are for humans and not for programmatic API use, you can hide them from the Swagger/ReDoc documentation by setting `include_in_schema=False` in the decorator.

```python
##Code
# Import FastAPI framework
from fastapi import FastAPI

# Import HTMLResponse so we can return HTML instead of JSON
from fastapi.responses import HTMLResponse


# Create FastAPI app instance
app = FastAPI()


# Dummy data acting like a temporary database
# list[dict] means:
# - outer structure is a list
# - each item inside is a dictionary
posts: list[dict] = [
    {
        "id": 1,
        "author": "Corey Schafer",
        "title": "FastAPI is Awesome",
        "content": "This framework is really easy to use and super fast.",
        "date_posted": "April 20, 2025",
    },

    {
        "id": 2,
        "author": "Jane Doe",
        "title": "Python is Great for Web Development",
        "content": "Python is a great language for web development, and FastAPI makes it even better.",
        "date_posted": "April 21, 2025",
    },
]


# Basic route example
# @app.get("/") means:
# whenever user visits "/" endpoint using GET request,
# this function will run

# @app.get("/")
# def home():
#     return {"message":"Hello World!"}


# response_class=HTMLResponse
# tells FastAPI that this route will return HTML

# include_in_schema=False
# means this endpoint will NOT appear in Swagger docs

@app.get("/", response_class=HTMLResponse, include_in_schema=False)

# Same function also works for "/posts"
# Multiple routes can point to same function
@app.get("/posts", response_class=HTMLResponse)

def home():

    # Returning HTML using f-string
    # posts[0] means first post from list
    # ['title'] accesses title key from dictionary

    return f"<h1>{posts[0]['title']}</h1>"


# API endpoint returning all posts
# FastAPI automatically converts Python list/dict into JSON
@app.get("/api/posts")

def get_posts():

    # Return all posts data
    return posts
```
