# Recipe App API

A fully functional REST API for managing recipes, built with Django REST Framework. Includes user authentication, recipe management with tags and ingredients, image upload, and full production deployment configuration.

## Features

- **User authentication** — token-based auth (register, login, manage profile)
- **Recipe management** — create, read, update, delete recipes with full detail
- **Tags & ingredients** — organize recipes and filter by assigned tags/ingredients
- **Image upload** — attach images to recipes via multipart form upload
- **API documentation** — auto-generated OpenAPI schema via drf-spectacular (Swagger UI)
- **Production-ready** — Dockerized with uWSGI app server and nginx reverse proxy

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3 |
| Framework | Django 3.2, Django REST Framework |
| Database | PostgreSQL 13 |
| Auth | Token Authentication |
| Docs | drf-spectacular (OpenAPI 3) |
| App server | uWSGI |
| Proxy | nginx |
| Containerization | Docker, Docker Compose |

## Getting Started

### Prerequisites

- [Docker](https://www.docker.com/) and Docker Compose installed

### Run locally (development)

```bash
# Clone the repository
git clone https://github.com/paulopes1/recipe-app-api.git
cd recipe-app-api

# Start the development stack (Django dev server + PostgreSQL)
docker-compose up
```

The API will be available at `http://localhost:8000`.

### Run in production

```bash
# Set required environment variables
cp .env.example .env
# Edit .env with your values: DB_NAME, DB_USER, DB_PASS, DJANGO_SECRET_KEY, DJANGO_ALLOWED_HOSTS

# Start the production stack (uWSGI + nginx + PostgreSQL)
docker-compose -f docker-compose-deploy.yml up -d
```

The API will be available at `http://localhost`.

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/user/create/` | Register a new user |
| POST | `/api/user/token/` | Obtain auth token |
| GET/PATCH | `/api/user/me/` | Manage authenticated user |
| GET/POST | `/api/recipe/recipes/` | List or create recipes |
| GET/PUT/PATCH/DELETE | `/api/recipe/recipes/{id}/` | Retrieve, update, or delete a recipe |
| POST | `/api/recipe/recipes/{id}/upload-image/` | Upload image for a recipe |
| GET/POST | `/api/recipe/tags/` | List or create tags |
| GET/PUT/PATCH/DELETE | `/api/recipe/tags/{id}/` | Manage a specific tag |
| GET/POST | `/api/recipe/ingredients/` | List or create ingredients |
| GET/PUT/PATCH/DELETE | `/api/recipe/ingredients/{id}/` | Manage a specific ingredient |

### Filtering

- `GET /api/recipe/recipes/?tags=1,2` — filter recipes by tag IDs
- `GET /api/recipe/recipes/?ingredients=3,4` — filter recipes by ingredient IDs
- `GET /api/recipe/tags/?assigned_only=1` — return only tags used in recipes
- `GET /api/recipe/ingredients/?assigned_only=1` — return only ingredients used in recipes

### API Documentation (Swagger UI)

```
http://localhost:8000/api/docs/
```

## Running Tests

```bash
docker-compose run --rm app sh -c "python manage.py test"
```

## Project Structure

```
recipe-app-api/
├── app/
│   ├── core/          # Custom user model, shared models (Tag, Ingredient)
│   ├── user/          # User registration and authentication endpoints
│   └── recipe/        # Recipe CRUD, image upload, filtering
├── proxy/             # nginx configuration for production
├── scripts/           # Entrypoint scripts (migrations, static files)
├── Dockerfile
├── docker-compose.yml            # Development stack
└── docker-compose-deploy.yml     # Production stack
```
