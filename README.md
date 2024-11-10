
# DJ: A Lightweight Django-Inspired Python Framework

## Overview
DJ is a Python web framework inspired by Django, designed for developers who prefer the flexibility and standardization of **SQLAlchemy** for ORM (Object Relational Mapping) and **Jinja** for templating. DJ is app-based, allowing you to organize each app independently with its own models, views, URLs, and forms. This modular structure simplifies project management and enhances scalability.

---

## Features
- **Modular, App-Based Architecture**: Each app can have its own models, views, URLs, templates, and forms.
- **Familiar MVC Structure**: Inspired by Django, making it easy for Django users to transition.
- **Standard Libraries**: Uses SQLAlchemy and Jinja, ensuring compatibility with popular libraries and standards.
- **Lightweight**: Minimal core without an enforced ORM or templating engine.
- **Extensibility**: Easy to integrate additional libraries or swap components due to the modular design.

---

## Installation
Install DJ and the required dependencies (SQLAlchemy and Jinja2) with:

```bash
pip install dj-framework SQLAlchemy Jinja2
```

---

## Folder Structure

In DJ, each app manages its own models, views, forms, and URLs, following a structure similar to Django’s app-based architecture.

```plaintext
my_project/
│
├── manage.py                   # Entry point for project commands like running the server, migrations, etc.
├── settings.py                 # Project-wide settings for database, middleware, and templates
├── urls.py                     # Root URL configuration file
│
├── main/                       # Main app of the project (could contain homepage or core logic)
│   ├── __init__.py
│   ├── models.py               # SQLAlchemy models for the main app
│   ├── views.py                # Views for the main app
│   ├── urls.py                 # URL patterns for the main app
│   ├── forms.py                # Forms for the main app (if needed)
│   └── templates/              # Templates specific to the main app
│       └── main/
│           └── base.html       # Example template for the main app
│
├── app1/                       # Example app (all apps follow this structure)
│   ├── __init__.py
│   ├── models.py               # SQLAlchemy models specific to app1
│   ├── views.py                # Views specific to app1
│   ├── urls.py                 # URL patterns specific to app1
│   ├── forms.py                # Forms specific to app1
│   └── templates/              # Templates specific to app1
│       └── app1/
│           └── index.html      # Example template for app1
│
├── app2/                       # Another example app
│   ├── __init__.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── forms.py
│   └── templates/
│       └── app2/
│           └── example.html
│
└── templates/                  # Shared templates across apps
    └── base.html               # Base template for all apps to extend
```

---

## Quick Start

### 1. Create a Project
Run the following command to create a new DJ project:

```bash
dj-admin startproject <project_name>
```

This creates a basic directory structure with a main app for your project. You can add more apps as needed.

### 2. Configure the Database
Set up your database URI in `settings.py`:

```python
# settings.py
DATABASE_URI = 'sqlite:///db.sqlite3'  # Replace with your database URI
```

### 3. Create Apps
Create a new app with the following command:

```bash
dj-admin startapp <app_name>
```

This creates an app with its own models, views, URLs, forms, and templates. 

### 4. Define Models
Each app manages its models independently using SQLAlchemy. Define models in each app’s `models.py` file:

```python
# app1/models.py
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String, unique=True)
```

### 5. Define Views
Each app’s views handle requests and responses. Place views in `views.py`:

```python
# app1/views.py
from dj.response import JsonResponse
from app1.models import User
from sqlalchemy.orm import Session

def user_list_view(request, db_session: Session):
    users = db_session.query(User).all()
    user_data = [{'id': user.id, 'name': user.name, 'email': user.email} for user in users]
    return JsonResponse(user_data)
```

### 6. Set Up URL Routing
Define URLs for each app in its own `urls.py`, and include them in the root `urls.py`:

```python
# app1/urls.py
from dj.routing import Route
from app1.views import user_list_view

urlpatterns = [
    Route('/users', user_list_view, name='user_list'),
]
```

```python
# urls.py (Root URL configuration)
from dj.routing import include, Route
from main import urls as main_urls
from app1 import urls as app1_urls

urlpatterns = [
    Route('/', include(main_urls)),
    Route('/app1/', include(app1_urls)),
]
```

### 7. Create Templates
Templates for each app are stored in `templates/<app_name>/`. Use Jinja for templating, allowing you to structure templates modularly and separately for each app.

---

## Core Concepts

### Models
- **SQLAlchemy Models**: Use SQLAlchemy to define models and manage schema.
- **CRUD Operations**: Standard SQLAlchemy sessions are used for database operations.
- **Migrations**: Use external tools like Alembic for migrations.

### Views
- **Function-Based Views**: DJ uses function-based views that handle incoming HTTP requests and return responses.
- **Response Types**: Use `JsonResponse`, `TemplateResponse`, or custom response types.

### Templates
- **Jinja2 Templates**: Leverage Jinja2 for rendering templates with flexibility.
- **App-Based Template Organization**: Each app has its own templates folder to keep templates organized.

### URL Routing
Each app’s `urls.py` file defines its routes, which are then included in the project’s root `urls.py`.

### Forms
If an app requires form handling, define forms in `forms.py` within each app. DJ doesn’t enforce a form library, allowing you to use libraries like **WTForms** or build custom forms.

### Middleware
Configure middleware in `settings.py` for additional request handling, authentication, or logging.

```python
MIDDLEWARE = [
    'middleware.auth.AuthenticationMiddleware',
]
```

---

## Settings

All configurations are set in `settings.py`:

- **DATABASE_URI**: Connection string for SQLAlchemy.
- **MIDDLEWARE**: List of middleware classes.
- **TEMPLATE_DIR**: Template directory for shared templates.
- **STATIC_DIR**: Directory for serving static files.

---

## Running the Development Server
Run the development server with:

```bash
dj-admin runserver
```

This starts a lightweight server at `http://127.0.0.1:8000`.

---

## Testing
DJ applications are compatible with testing frameworks like **pytest** and **unittest**. Since DJ uses SQLAlchemy and Jinja2, no special setup is required beyond standard Python testing practices.

---

## Contributing
To contribute to DJ:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/YourFeature`).
3. Commit your changes (`git commit -am 'Add YourFeature'`).
4. Push to the branch (`git push origin feature/YourFeature`).
5. Open a pull request.

---

## License
DJ is released under the MIT License.
