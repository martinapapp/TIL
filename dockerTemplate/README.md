# Docker Template

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)

A general-purpose Docker template for containerizing any web application — just swap the base image and commands for your stack.

## Index

- [About](#about)
- [Usage](#usage)
- [Development](#development)
- [Contribution](#contribution)
- [License](#license)

---

## About

A ready-to-use Docker setup you can drop into any project. The main goal was to learn how to:

- Containerize a web application using **Docker**
- Write an efficient **Dockerfile** with proper layer caching
- Manage multi-service setups with **Docker Compose**
- Use a **`.dockerignore`** to keep images lean
- Handle environment variables and secrets safely
- Harden containers by running as a **non-root user**

---

## Usage

### Installation

1. Make sure [Docker Desktop](https://www.docker.com/products/docker-desktop/) is installed and running.
2. Clone or copy this template into your project root.
3. Update the base image, install command, and `CMD` in `Dockerfile` to match your stack (see table in [Build](#build)).
4. Update the port mapping in `docker-compose.yml` to match your app.
5. Add any environment variables to a `.env` file (never commit this).

### Commands

Here are the main Docker commands to know:
```bash
docker build -t my-app:latest . // Build the image

docker run -p 3000:3000 my-app:latest // Run a container

docker compose up // Start with Docker Compose

docker compose up --build // Rebuild and start

docker compose down // Stop all running services
```

---

## Development

### Pre-Requisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- A text editor (e.g. VS Code)
- Basic knowledge of the terminal
- Your app's source code ready in the project folder

### File Structure

| No | File Name | What it does |
| --- | --- | --- |
| 1 | `Dockerfile` | Defines how to build the container image for your app |
| 2 | `docker-compose.yml` | Orchestrates one or more services (app, database, etc.) |
| 3 | `.dockerignore` | Tells Docker which files to exclude from the image build |
| 4 | `README.md` | You're reading it! |

### Build

The `Dockerfile` works in layers. Docker caches each layer, so dependency files are copied first — meaning if your source code changes but your dependencies don't, Docker skips the slow install step and reuses the cache. The `docker-compose.yml` wraps this with port mapping, environment variables, and optional extras like a database service. Swap out the base image and commands in the table below to adapt it to any stack:

| Stack | Base image | Install command | Start command |
| --- | --- | --- | --- |
| Node.js | `node:22-alpine` | `npm ci --omit=dev` | `node src/server.js` |
| Python | `python:3.12-slim` | `pip install -r requirements.txt` | `python app.py` |
| Go | `golang:1.22-alpine` | `go build -o app .` | `./app` |
| Static | `nginx:alpine` | *(copy files)* | *(nginx default)* |

---

## Contribution

1. Found a bug? Open an issue and I'll try to fix it.
2. Advice? If you have ideas for improving the template or adding new stack examples, let me know!

### Guideline

Keep changes focused — one stack or one fix per pull request. Add a comment in the relevant file explaining what you changed and why, so the template stays easy to follow for learners.

---

## License

Feel free to use this for your own practice! **MIT** License.