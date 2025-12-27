# Blog API

This project is a RESTful API for a blog platform built with Django and Django Rest Framework (DRF). It provides functionalities for user registration, authentication, blog post management (including categories and tags), and comments.

The live API documentation, powered by Swagger UI, can be accessed here: [https://blog-api-b3t1.onrender.com/](https://blog-api-b3t1.onrender.com/)

## Features

- **User Management:** User registration, JWT-based authentication (login), and profile management.
- **Blog Post Management:** CRUD operations for blog posts.
- **Categories and Tags:** Full management of categories and tags for blog posts.
- **Commenting System:** Users can comment on blog posts.
- **Search and Filtering:** Filter posts by category and author, and search by content.
- **API Documentation:** Auto-generated interactive API documentation using `drf-yasg`.
- **Admin Interface:** Django admin for managing all data.

## Prerequisites

- Python (3.7+)
- [Poetry](https://python-poetry.org/docs/) for dependency management (or you can use `pip` with the `requirements.txt`)

## Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/yourusername/Blog-API.git
    cd Blog-API
    ```

2.  **Set up the environment and install dependencies:**

    *   **Using Poetry:**
        ```bash
        poetry install
        poetry shell
        ```

    *   **Using `pip` and `venv`:**
        ```bash
        python3 -m venv venv
        source venv/bin/activate  # On Windows: venv\Scripts\activate
        pip install -r requirements.txt
        ```

3.  **Database Migrations:**
    ```bash
    python manage.py migrate
    ```

4.  **Create a Superuser (for admin access):**
    ```bash
    python manage.py createsuperuser
    ```

5.  **Run the Development Server:**
    ```bash
    python manage.py runserver
    ```
    The API will be available at `http://127.0.0.1:8000/`.

## Running Tests

To run the automated tests, execute the following command:
```bash
python manage.py test
```

## API Endpoints

The API is structured into three main applications: `users`, `blog`, and `comment`. All API endpoints are prefixed with `/api/`.

### Authentication

This API uses JSON Web Tokens (JWT) for authentication. To access protected endpoints, you must first obtain a token from the `/api/users/login/` endpoint and then include it in the `Authorization` header of your requests as `Bearer <your_token>`.

### Users (`/api/users/`)

-   `POST /register/`: Register a new user.
    -   **Body:** `username`, `email`, `password`
-   `POST /login/`: Obtain JWT access and refresh tokens.
    -   **Body:** `email`, `password`
-   `GET, PUT /profile/`: View or update the authenticated user's profile.
    -   **Permissions:** Authenticated user.
    -   **Body (for PUT):** `bio`, `birthdate` (optional)

### Blog (`/api/blog/`)

-   `GET /posts/`: List all blog posts.
-   `POST /posts/create/`: Create a new blog post.
    -   **Permissions:** Authenticated user.
    -   **Body:** `title`, `content`, `category` (ID), `tags` (List of IDs).
-   `GET /posts/<id>/`: Retrieve a single blog post.
-   `PUT /posts/update/<id>/`: Update a blog post.
    -   **Permissions:** Author of the post.
-   `DELETE /posts/delete/<id>/`: Delete a blog post.
    -   **Permissions:** Author of the post.
-   `GET /posts/category/<category_id>/`: List posts belonging to a specific category.
-   `GET /posts/author/<author_id>/`: List posts by a specific author.
-   `GET /posts/search/?search=<query>`: Search for blog posts.

#### Categories & Tags

The `blog` app also exposes viewsets for managing categories and tags.

-   `GET, POST /categories/`: List all categories or create a new one.
-   `GET, PUT, DELETE /categories/<id>/`: Retrieve, update, or delete a specific category.
-   `GET, POST /tags/`: List all tags or create a new one.
-   `GET, PUT, DELETE /tags/<id>/`: Retrieve, update, or delete a specific tag.

### Comments (`/api/comment/`)

The `comment` app uses a `ViewSet`, so it provides the following endpoints under the `/api/comment/comments/` path.

-   `GET /comments/`: List all comments.
-   `POST /comments/`: Create a new comment.
    -   **Permissions:** Authenticated user.
    -   **Body:** `post` (ID of the blog post), `content`
-   `GET /comments/<id>/`: Retrieve a single comment.
-   `PUT /comments/<id>/`: Update a comment.
    -   **Permissions:** Author of the comment.
-   `DELETE /comments/<id>/`: Delete a comment.
    -   **Permissions:** Author of the comment.