# Laravel Docker with SQL Server (MSSQL)

A complete Docker development environment for Laravel with SQL Server using PDO sqlsrv driver.

## Stack

- PHP 8.3 (FPM) with pdo_sqlsrv extension
- Nginx (Alpine)
- SQL Server 2022 (Developer Edition)
- Composer

## Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)

## Installation

### 1. Clone the repository
```bash
git clone https://github.com/g-marinescu/laravel-docker-mssql.git
cd laravel-docker-mssql
```

### 2. Copy environment files
```bash
cp .env.example .env
cp src/.env.example src/.env
```

### 3. Configure Laravel database (src/.env)

Edit `src/.env` and set the database section:
```env
DB_CONNECTION=sqlsrv
DB_HOST=mssql
DB_PORT=1433
DB_DATABASE=laravel_db
DB_USERNAME=sa
DB_PASSWORD=YourStrong@Password123
```

### 4. Build and start containers
```bash
docker compose build
docker compose up -d
```

### 5. Install Laravel dependencies
```bash
docker compose exec php composer install
```

### 6. Generate application key
```bash
docker compose exec php php artisan key:generate
```

### 7. Create the database
```bash
docker compose exec mssql /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P "YourStrong@Password123" -C
```

In the SQL prompt:
```sql
CREATE DATABASE laravel_db;
GO
exit
```

### 8. Run migrations
```bash
docker compose exec php php artisan migrate
```

### 9. Access the application

Open http://localhost:8081 in your browser.

## Useful Commands

| Command | Description |
|---------|-------------|
| `docker compose up -d` | Start containers |
| `docker compose down` | Stop containers |
| `docker compose ps` | View container status |
| `docker compose exec php bash` | Enter PHP container shell |
| `docker compose exec php php artisan migrate` | Run migrations |
| `docker compose exec php composer require package` | Install packages |
| `docker compose logs -f` | View logs in real-time |

## Port Configuration

If you have port conflicts, edit `.env` in the root folder:
```env
WEB_PORT=8081      # Change if 8081 is in use
MSSQL_PORT=1433    # Change if 1433 is in use
```

**Note:** If you change `MSSQL_PORT`, also update `DB_PORT` in `src/.env`.

## License

MIT
