
# 📋 Task Management Application

> A modular REST API built with **Python & FastAPI** for managing tasks and users — clean architecture, domain-driven structure, JWT authentication.

---

## 📁 Project Structure

```
Task Management Application/
├── src/
│   ├── task/
│   │   ├── __init__.py
│   │   ├── controller.py
│   │   ├── dtos.py
│   │   ├── model.py
│   │   └── routers.py
│   ├── user/
│   │   ├── __init__.py
│   │   ├── controller.py
│   │   ├── dtos.py
│   │   ├── model.py
│   │   └── routers.py
│   └── utils/
│       ├── __init__.py
│       ├── constain.py
│       ├── db.py
│       ├── helper.py
│       └── settings.py
├── .env
├── .gitignore
├── main.py
├── README.md
└── requirement.txt
```

---

## 🗂️ Folder & File Breakdown

### 🔧 Root Level

| File | Description |
|------|-------------|
| `main.py` | Application entry point — creates the FastAPI app, registers all routers, and starts the Uvicorn server |
| `.env` | Environment variables (DB URL, secret key, debug flag). **Never commit this file** |
| `.gitignore` | Excludes sensitive/generated files from Git (`.env`, `__pycache__`, `.venv`, etc.) |
| `requirement.txt` | All Python dependencies — install with `pip install -r requirement.txt` |
| `README.md` | Project documentation (this file) |

---

### 📦 `src/` — Source Code

All application logic lives here, split into domain packages.

---

#### ✅ `src/task/` — Task Domain

Handles everything related to tasks: creation, retrieval, updating, and deletion.

| File | Role |
|------|------|
| `__init__.py` | Marks the folder as a Python package |
| `model.py` | SQLAlchemy ORM model — defines the `tasks` table (id, title, description, status, due_date, owner_id, created_at) |
| `dtos.py` | Pydantic schemas for request validation and response serialisation (`CreateTaskDTO`, `UpdateTaskDTO`, `TaskResponseDTO`) |
| `controller.py` | Business logic — create, fetch, update, delete tasks; raises HTTP exceptions on invalid operations |
| `routers.py` | FastAPI route handlers — maps HTTP endpoints to controller functions |

**Task endpoints:**

```
POST   /tasks          → Create a new task
GET    /tasks          → List all tasks (with filters)
GET    /tasks/{id}     → Get a single task
PUT    /tasks/{id}     → Update a task
DELETE /tasks/{id}     → Delete a task
```

---

#### 👤 `src/user/` — User Domain

Handles user registration, authentication, and profile management.

| File | Role |
|------|------|
| `__init__.py` | Marks the folder as a Python package |
| `model.py` | SQLAlchemy ORM model — defines the `users` table (id, username, email, hashed_password, is_active, created_at) |
| `dtos.py` | Pydantic schemas (`CreateUserDTO`, `LoginDTO`, `UserResponseDTO`) |
| `controller.py` | Business logic — register user, verify password, return profile |
| `routers.py` | FastAPI route handlers for user-related endpoints |

**User endpoints:**

```
POST   /users/register → Register a new user
POST   /users/login    → Authenticate and receive JWT token
GET    /users/me       → Get current user profile
```

---

#### 🛠️ `src/utils/` — Shared Utilities

Cross-cutting concerns shared across all domains.

| File | Role |
|------|------|
| `__init__.py` | Marks the folder as a Python package |
| `db.py` | Creates the SQLAlchemy `engine` and `SessionLocal` factory; exposes `get_db()` dependency for FastAPI |
| `settings.py` | Loads and validates `.env` variables using Pydantic `BaseSettings` — exposes a typed `settings` object |
| `constain.py` | App-wide constants (task statuses, role names, pagination defaults, error messages) |
| `helper.py` | Reusable functions: password hashing, JWT generation, token verification, pagination |

---

## 🏗️ Architecture

```
HTTP Request
     │
     ▼
 routers.py        ← Route definitions & dependency injection
     │
     ▼
controller.py      ← Business logic, validation, error handling
     │
     ▼
  model.py         ← ORM queries & database interaction
     │
     ▼
  Database (PostgreSQL / SQLite)

  utils/ ─ used by all layers
  ├── db.py        → DB session management
  ├── settings.py  → Config from .env
  ├── helper.py    → Auth & utility functions
  └── constain.py  → Shared constants
```

**Request flow:**
`Client → routers.py → controller.py → model.py → DB → dtos.py (response) → Client`

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- pip
- PostgreSQL or SQLite

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/task-management-app.git
cd task-management-app

# 2. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirement.txt

# 4. Set up environment variables
cp .env.example .env
# Edit .env with your actual values

# 5. Start the development server
uvicorn main:app --reload
```

The API will be available at `http://localhost:8000`  
Interactive docs at `http://localhost:8000/docs`

---
## DataBase Updation using Alambic 
`Alembic is the industry-standard database migration tool for SQLAlchemy, commonly used in FastAPI applications to safely update, track, and roll back relational database schemas over time.`


<p align="center">
  <img src="images\image.png" width="600" alt="Screenshot">
</p>

```
```
## ⚙️ Environment Variables

Create a `.env` file in the root directory:

```env
DATABASE_URL=postgresql://user:password@localhost/taskdb
SECRET_KEY=your-super-secret-key-here
DEBUG=True
ACCESS_TOKEN_TTL=30
```

| Variable | Description |
|----------|-------------|
| `DATABASE_URL` | Full DB connection string |
| `SECRET_KEY` | Random string for signing JWT tokens — keep this secret |
| `DEBUG` | Enable verbose error output during development |
| `ACCESS_TOKEN_TTL` | JWT expiry in minutes (default: 30) |

---
