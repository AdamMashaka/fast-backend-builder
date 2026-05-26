# fast-backend-builder

**fast-backend-builder** is a project generator and manager for **FastAPI** with **Tortoise ORM** & **Aerich**.

💡 **FastAPI Backend Builder** – A lightweight library to help you quickly scaffold and build FastAPI projects as a starter kit.

---

## 📌 Overview
`fast-backend-builder` is designed for **rapid web application development** with FastAPI.  
It comes bundled with tools and integrations to help developers create well-structured, maintainable, and production-ready FastAPI applications in no time.

The goal is to provide a **one-stop solution** to accelerate prototyping and simplify production deployment.

🔗 **Source Code:** [GitHub Repository](https://github.com/jay-ludanga/fast-backend-builder)

---

## ✨ Features

- 🚀 **CRUD Base with GraphQL (Strawberry)** – Auto-generate and manage CRUD endpoints with GraphQL support  
- 🔐 **Authentication with JWT** – Built-in secure authentication and token management  
- ⚡ **Redis Integration** – Caching, sessions, and queue support with Redis  
- 📂 **File Storage with MinIO** – Easy file upload and attachment handling  
- 🔗 **ESB Integrations** – Simplified enterprise service bus support  
- 📬 **BullMQ Integration** – Notifications and queue management out of the box  

---

---

## 🚀 Quick Start

Create FastAPI App for more visit https://fastapi.tiangolo.com/

## ⚙️ Installation

```bash

pip install fast-backend-builder
```

### 🚀 Setting Up a Custom User Model in FastAPI with Tortoise ORM + FastAPI-Builder

This guide explains how to create a package module for your **User model**, extend it from `AbstractUser`, configure it in Tortoise ORM, and generate GraphQL + migrations.

---

## 📂 Project Structure

```bash

myapp
  ├── mymodel_package
  │   ├── __init__.py
  │   └── models
  │       └── user.py
  ├── config
  │   ├── __init__.py
  │   └── tortoise.py
  ├── main.py
  └── ...
```

👤 1. Create Your Custom User Model

Inside mymodel_package/models/user.py:

```python
from fast_backend_builder.models.base_models import AbstractUser


class User(AbstractUser):
    """Custom application user model extending AbstractUser."""
    pass
```

⚙️ 2. Set Your User Model in main.py (your main FastAPI file)

At the top of your main.py (before any model imports):

```python
from fast_backend_builder.utils.config import set_user_model
from mymodel_package.models.user import User

# Register the User model so fast_backend_builder can resolve it
set_user_model(User, "models.User")
```

🗄️ 3. Create Tortoise ORM Config

Inside config/tortoise.py:
```python
from decouple import config

db_url = f"postgres://{config('DB_USER')}:{config('DB_PASSWORD')}@{config('DB_HOST')}:{config('DB_PORT')}/{config('DB_NAME')}"

TORTOISE_ORM = {
    "connections": {"default": db_url},
    "apps": {
        "models": {
            "models": [
                "fast_backend_builder.models",   # built-in models
                "mymodel_package.models",    # your custom models
                "aerich.models",             # migration tracking
            ],
            "default_connection": "default",
        },
    },
    "use_tz": True,  # Enable timezone-aware datetimes
    "timezone": "Africa/Dar_es_Salaam",  # Set to EAT (Dar es Salaam)
}
```

🔧 4. Generate CRUD APIs via GraphQL

Run the following to scaffold GraphQL CRUD APIs:
```bash
# For your custom User model
graphql gen:crud-api user_management --module-package=mymodel_package.models --model User

# For fast_backend_builder built-in models
graphql gen:crud-api user_management --module-package=fast_backend_builder.models --model Group,Permission,Headship
graphql gen:crud-api workflow --module-package=fast_backend_builder.models --model Workflow,WorkflowStep,Transition,Evaluation
```
For your Models
```bash
# 
graphql gen:crud-api <module-name> --model Model1,Model2,Model3

# generating graphql schemas with attachments

graphql gen:crud-api <module-name> --model ModelName --with-attachment

# generating graphql schemas with transitions

graphql gen:crud-api <module-name> --model ModelName --with-transition
```

📦 5. Initialize Aerich (DB migrations)
```bash
# Initialize Aerich with your Tortoise ORM config
aerich init -t config.tortoise.TORTOISE_ORM

# Create initial migration & database tables
aerich init-db
```

## 📖 Documentation
Coming soon...

---

## 🤝 Contributing
Contributions, issues, and feature requests are welcome!  
Feel free to check the [issues page](https://github.com/jay-ludanga/fast-backend-builder/issues).

---

## 📜 License
This project is licensed under the **Other/Proprietary License**.


# Thanks

Thanks for all the contributors that made this library possible,
also a special mention to Japhary Juma, Shija Ntula and Juma Nassoro.
