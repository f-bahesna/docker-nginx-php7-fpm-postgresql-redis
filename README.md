# 🐳 Docker Environment Setup: Nginx, PHP, PostgreSQL & Redis

This repository provides setting Docker Compose configurations for Laravel Applications

## 🚀 Supported Configurations

| Configuration | PHP Version | Base Image | Supported Laravel Version | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `php7-fpm-pgsql-redis` | **7.4** | Standard | v.6.18.3+ | Ideal for legacy or older projects. |
| `php8-fpm-alpine-pgsql-redis` | **8.0** | Alpine | v.8.7.3+ | Uses the minimal Alpine image for a smaller footprint. |

-----

## 🛠️ Prerequisites

  * **Docker:** Latest version of Docker Engine.
  * **Docker Compose:** Usually bundled with Docker Desktop.

-----

## ⚙️ Setup and Usage

### 1\. Configure Environment

Copy the example environment file for your chosen configuration:

```bash
# Example for PHP 8.0 configuration
cp docker-compose.php8-fpm-alpine-pgsql-redis.yml docker-compose.yml
```

### 2\. Update Laravel's `.env` File

Ensure your Laravel application's `.env` file is configured to connect to the services running inside Docker.

| Service | Variable | Value |
| :--- | :--- | :--- |
| **PostgreSQL** | `DB_HOST` | `pgsql` |
| **Redis** | `REDIS_HOST` | `redis` |
| **Nginx/PHP** | (N/A) | (N/A) |

### 3\. Build and Run the Containers

From the root directory of your project, use Docker Compose to start all services:

```bash
docker-compose up -d --build
```

  * The `--build` flag ensures containers are rebuilt if necessary (recommended for the first run).
  * The `-d` flag runs the containers in **detached mode** (background).

### 4\. Install PHP Dependencies

You'll need to run Composer inside the PHP container. Replace `php8-fpm` with your container's service name if using a different setup.

```bash
# Access the PHP container's shell
docker-compose exec php8-fpm bash

# Run Composer installation
composer install

# Once done, exit the container
exit
```

Your application should now be accessible via your local machine's web browser, typically at `http://localhost`.

-----

## ⚠️ Troubleshooting Connection Errors

If you encounter connection problems, especially with PostgreSQL (`could not connect to server`) or Redis (`redis connection refused`), the issue is often related to how Docker Desktop handles network resolution on certain operating systems.

### 🍎 For macOS Users (Recommended Fix)

If your Laravel application is running **outside** Docker (i.e., you are only running the services like DB/Redis in Docker), you must use the special Docker hostname to access the services from your host machine.

Change the host value in your Laravel `.env` file:

| Service | Old Host | **New Host (for macOS)** |
| :--- | :--- | :--- |
| **PostgreSQL** | `localhost` or `127.0.0.1` | `docker.for.mac.localhost` |
| **Redis** | `localhost` or `127.0.0.1` | `docker.for.mac.localhost` |

**Example `.env` Snippet (macOS):**

```
DB_CONNECTION=pgsql
DB_HOST=docker.for.mac.localhost
DB_PORT=5432
DB_DATABASE=laravel

REDIS_HOST=docker.for.mac.localhost
REDIS_PASSWORD=null
REDIS_PORT=6379
```

### 🖥️ General Linux/Windows Users

For standard Linux and Windows environments, the services should typically be accessible using `localhost` or `127.0.0.1` if you are accessing them from your host machine.

However, if your Laravel application is also running **inside another Docker container**, the correct hostnames are the service names defined in `docker-compose.yml` (e.g., `pgsql` and `redis`), as outlined in **Step 2** above.

-----

## 🛑 Stopping and Cleanup

### Stop Containers

To stop the running containers without removing their data:

```bash
docker-compose stop
```

### Stop and Remove Containers/Networks

To stop the containers and remove the containers and networks (but keep persistent volumes):

```bash
docker-compose down
```

### Full Cleanup (Remove Volumes/Data)

To remove all containers, networks, and the **persistent data volumes** (PostgreSQL data, etc.):

```bash
docker-compose down -v
```

> ⚠️ **Warning:** This command will permanently delete all data stored in the database and other volumes. Use with caution.
