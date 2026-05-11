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

## Challenge 2: Add a Database

In this challenge, I added a PostgreSQL database service using the `postgres:15` image.

The database uses environment variables for:

- Username: `app_user`
- Password: `app_password`
- Database name: `app_db`

The backend service depends on the database service, so the backend starts after the database container starts.

Note: The frontend uses port `3001` instead of `3000` because port `3000` was already occupied in the Poridhi lab environment.
![alt text](image-3.png)

## Challenge 3: Database Persistence

In this challenge, I added a named Docker volume to PostgreSQL so that database data remains persistent even after containers are removed.
![alt text](image-4.png)
Volume used:

- `postgres_data`

Mount location inside container:

- `/var/lib/postgresql/data`

To verify persistence, I:

1. Created a sample table
2. Inserted test data
   ![alt text](image-5.png)
3. Removed containers using `docker compose down`
   ![alt text](image-6.png)
4. Started containers again
   ![alt text](image-7.png)
5. Verified that the data still existed
   ![alt text](image-8.png)
