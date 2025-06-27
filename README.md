# Orc Salesman Project

A project with a Symfony (PHP) backend and an Angular frontend, containerized with Docker.
The backend is served by [FrankenPHP](https://frankenphp.dev/).

## Prerequisites

- Docker
- Docker Compose

## Project Structure

```
├── backend/      # Symfony application
├── frontend/     # Angular application
└── docker-compose.yml
```

## Getting Started

1.  Ensure your Symfony project is in the `backend/` directory and your Angular project is in the `frontend/` directory.
2.  Run the application with Docker Compose from the project root:
    ```bash
    docker-compose up --build -d
    ```

Your applications will be available at:
- **Frontend (Angular)**: http://localhost:4200
- **Backend (Symfony API)**: http://localhost:8080