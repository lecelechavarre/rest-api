# REST API with Dockerized Deployment

This project provides a robust backend REST API built with Python/Flask, featuring JWT authentication, a PostgreSQL database, Docker containerization, and a basic CI/CD pipeline using GitHub Actions.

## Table of Contents
- [Project Structure](#project-structure)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Local Development with Docker Compose](#local-development-with-docker-compose)
  - [Running Tests](#running-tests)
- [API Endpoints](#api-endpoints)
- [CI/CD Pipeline](#ci/cd-pipeline)
- [Environment Variables](#environment-variables)

## Project Structure

```
rest_api_project/
├── app/
│   ├── __init__.py
│   ├── config.py
│   ├── models.py
│   └── routes.py
├── .github/
│   └── workflows/
│       └── main.yml
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── run.py
└── README.md
```

## Features
- **Python/Flask Backend**: Lightweight and flexible web framework.
- **JWT Authentication**: Secure user authentication using JSON Web Tokens.
- **PostgreSQL Database**: Robust relational database for data persistence.
- **Docker Containerization**: Easy setup and consistent environment across development and production.
- **GitHub Actions CI/CD**: Automated testing and build process.

## Prerequisites
Before you begin, ensure you have the following installed on your system:
- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Docker Desktop](https://www.docker.com/products/docker-desktop) (includes Docker Engine and Docker Compose)

## Getting Started

### Local Development with Docker Compose

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/rest_api_project.git
    cd rest_api_project
    ```

2.  **Build and run the Docker containers:**
    This command will build the `api` service image, create the `db` service, and start both containers. It also sets up a persistent volume for your PostgreSQL data.
    ```bash
    docker-compose up --build -d
    ```
    The `-d` flag runs the containers in detached mode.

3.  **Verify containers are running:**
    ```bash
    docker-compose ps
    ```
    You should see `api` and `db` services listed with `Up` status.

4.  **Access the API:**
    The Flask API will be accessible at `http://localhost:5000`.

    **Example API Calls (using `curl` or Postman/Insomnia):**

    *   **Register a new user:**
        ```bash
        curl -X POST -H "Content-Type: application/json" -d '{"username": "testuser", "password": "testpassword"}' http://localhost:5000/register
        ```

    *   **Login and get JWT token:**
        ```bash
        curl -X POST -H "Content-Type: application/json" -d '{"username": "testuser", "password": "testpassword"}' http://localhost:5000/login
        ```
        This will return a JWT access token. Copy this token.

    *   **Access a protected endpoint:**
        Replace `YOUR_ACCESS_TOKEN` with the token obtained from the login step.
        ```bash
        curl -X GET -H "Authorization: Bearer YOUR_ACCESS_TOKEN" http://localhost:5000/protected
        ```

5.  **Stop the containers:**
    ```bash
    docker-compose down
    ```
    This will stop and remove the containers, networks, and volumes (unless specified otherwise).

### Running Tests

(Note: A basic test setup is included in `requirements.txt` with `pytest`, but no actual test files are provided in this scaffold. You would add your test files in a `tests/` directory.)

To run tests locally (after building the Docker image or installing dependencies directly):

1.  **Install dependencies (if not using Docker for tests):**
    ```bash
    pip install -r requirements.txt
    ```

2.  **Execute tests:**
    ```bash
    pytest
    ```

## API Endpoints

| Endpoint      | Method | Description                                  | Authentication Required |
| :------------ | :----- | :------------------------------------------- | :---------------------- |
| `/register`   | `POST` | Register a new user                          | No                      |
| `/login`      | `POST` | Authenticate user and get JWT access token   | No                      |
| `/protected`  | `GET`  | Example protected endpoint (requires JWT)    | Yes                     |

## CI/CD Pipeline

The project includes a basic CI/CD pipeline configured with GitHub Actions (`.github/workflows/main.yml`).

-   **`test` job**: Runs on every `push` and `pull_request` to the `main` branch. It checks out the code, sets up Python, installs dependencies, and runs placeholder tests.
-   **`build` job**: Depends on the `test` job. It builds the Docker image for the application. In a production setup, this job would also push the image to a container registry (e.g., Docker Hub, AWS ECR).

## Environment Variables

These variables can be set in your `docker-compose.yml` (for local development) or in your deployment environment (e.g., Kubernetes secrets, CI/CD environment variables) for production.

| Variable             | Description                                                               | Default (Local)                                |
| :------------------- | :------------------------------------------------------------------------ | :--------------------------------------------- |
| `SECRET_KEY`         | Flask application secret key for session management and security.         | `dev_secret_key`                               |
| `JWT_SECRET_KEY`     | Secret key for signing JWT tokens.                                        | `dev_jwt_secret_key`                           |
| `DATABASE_URL`       | Connection string for the PostgreSQL database.                            | `postgresql://user:password@db:5432/mydatabase`|

**Note**: For production deployments, ensure these keys are strong, randomly generated, and securely managed (e.g., using environment variables or a secret management service).
