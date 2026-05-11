# Docker Compose Assignment

This assignment builds a small multi-service application step by step using Docker Compose.

## Services Used

- Backend: Nginx
- Frontend: Nginx
- Database: PostgreSQL
- Cache: Redis

## Challenge 1: Launch the Basic App

In this challenge, I created two services:

- `backend` using `nginx:latest`, exposed on host port `8080`
- `frontend` using `nginx:latest`, exposed on host port `3000`

The frontend depends on the backend, so it starts after the backend.

# Note: In the lab environment, port 3000 was already occupied by an internal Node.js process. Therefore, port 3001 was used for the frontend service instead.

<!-- Docker PS -->

![alt text](image.png)

<!-- Frontend -->

![alt text](image-1.png)

<!-- Backend -->

![alt text](image-2.png)
