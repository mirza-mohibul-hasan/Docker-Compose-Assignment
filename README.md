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
- `frontend` using `nginx:latest`, exposed on host port `3001`

The frontend depends on the backend, so it starts after the backend.

> **Note:** In the Poridhi lab environment, port `3000` was already occupied by an internal Node.js process. Therefore, port `3001` was used for the frontend service instead.

<!-- Docker PS -->

![Docker Compose services running in the terminal](images/challenge1-ps.png)

<!-- Frontend -->

![Frontend service running in the browser](images/challenge1-frontend.png)

<!-- Backend -->

![Backend service running in the browser](images/challenge1-backend.png)

## Challenge 2: Add a Database

In this challenge, I added a PostgreSQL database service using the `postgres:15` image.

The database uses environment variables for:

- Username: `app_user`
- Password: `app_password`
- Database name: `app_db`

The backend service depends on the database service, so the backend starts after the database container starts.
![PostgreSQL database service configuration](images/challenge2-ps.png)

## Challenge 3: Database Persistence

In this challenge, I added a named Docker volume to PostgreSQL so that database data remains persistent even after containers are removed.
![Persistent PostgreSQL volume configuration](images/challenge3-persistence-config.png)
Volume used:

- `postgres_data`

Mount location inside container:

- `/var/lib/postgresql/data`

To verify persistence, I:

1. Created a sample table
2. Inserted test data
   ![Sample table with inserted test data](images/challenge3-persistence-before.png)
3. Removed containers using `docker compose down`
   ![Containers stopped with docker compose down](images/challenge3-persistence-after-down.png)
4. Started containers again
   ![Containers started again after restart](images/challenge3-persistence-after-restart.png)
5. Verified that the data still existed
   ![Persistent data still available after restart](images/challenge3-persistence-verify.png)

## Challenge 4: Wait Until Database Is Ready

In this challenge, I added a health check for PostgreSQL using the `pg_isready` command.

This ensures that the backend service waits until PostgreSQL is fully ready and accepting connections before starting.

Health check configuration:
![PostgreSQL health check configuration](images/challenge4-healthcheck-config.png)

- Command: `pg_isready`
- Interval: `5s`
- Timeout: `5s`
- Retries: `5`
  ![Backend waiting for PostgreSQL health check](images/challenge4-healthcheck-wait.png)
  The backend service now depends on the database service being healthy instead of only waiting for the container to start.

## Challenge 5: Add Redis for Caching

In this challenge, I added a Redis caching service using the `redis:7` image.

A Redis health check was added using:

```bash
redis-cli ping
```

The backend service now waits until both:

- PostgreSQL is healthy
- Redis is healthy
  ![Backend waiting for PostgreSQL and Redis health checks](images/challenge5-redis-healthcheck.png)

## Challenge 6: Survive Docker Restart

In this challenge, I configured automatic container recovery using:

```yaml
restart: unless-stopped
```

![Docker restart policy configuration](images/challenge6-docker-restart.png)
This ensures that all services automatically restart after:

- Docker daemon restart
- System reboot
  ![Services restarting after Docker daemon restart](images/challenge6-docker-restart-services.png)
