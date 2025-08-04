# WordPress Docker Compose Setup

This Docker Compose configuration provides a complete WordPress development environment with MariaDB and Redis.

## Services Included

- **WordPress** (wordpress:latest) - Main WordPress application
- **MariaDB** (mariadb:10.11) - Database server
- **Redis** (redis:7-alpine) - Caching and session storage
- **phpMyAdmin** (optional) - Database management interface
- **Redis Commander** (optional) - Redis management interface

## Quick Start

1. **Copy the environment file:**
   ```bash
   cp .env.example .env
   ```

2. **Edit the `.env` file and change the default passwords:**
   ```bash
   nano .env
   ```

3. **Start the services:**
   ```bash
   docker-compose up -d
   ```

4. **Access your WordPress site:**
   - WordPress: http://localhost:8080
   - phpMyAdmin: http://localhost:8081
   - Redis Commander: http://localhost:8082

## Services Details

### WordPress
- **Port:** 8080 (configurable via WORDPRESS_PORT in .env)
- **Volume:** WordPress files are persisted in `wordpress_data` volume
- **wp-content:** Mapped to local `./wp-content` directory for theme/plugin development

### MariaDB
- **Port:** 3306 (configurable via MARIADB_PORT in .env)
- **Volume:** Database data persisted in `mariadb_data` volume
- **Credentials:** Configurable via .env file

### Redis
- **Port:** 6379 (configurable via REDIS_PORT in .env)
- **Volume:** Redis data persisted in `redis_data` volume
- **Configuration:** Optimized for WordPress caching

## WordPress Redis Configuration

To use Redis for object caching, install the Redis Object Cache plugin:

1. Install the plugin via WordPress admin or WP-CLI
2. The necessary Redis configuration is already added to `WORDPRESS_CONFIG_EXTRA`

## Useful Commands

### Start services
```bash
docker-compose up -d
```

### Stop services
```bash
docker-compose down
```

### View logs
```bash
docker-compose logs -f wordpress
docker-compose logs -f mariadb
docker-compose logs -f redis
```

### Restart a specific service
```bash
docker-compose restart wordpress
```

### Access WordPress container shell
```bash
docker-compose exec wordpress bash
```

### Access MariaDB shell
```bash
docker-compose exec mariadb mysql -u wordpress -p
```

### Backup database
```bash
docker-compose exec mariadb mysqldump -u wordpress -p wordpress > backup.sql
```

### Restore database
```bash
docker-compose exec -T mariadb mysql -u wordpress -p wordpress < backup.sql
```

## Development Tips

1. **Local wp-content mapping:** The `./wp-content` directory is mapped to the container, allowing you to develop themes and plugins locally.

2. **Database persistence:** Database data is stored in Docker volumes, so it persists between container restarts.

3. **Redis caching:** Redis is configured for optimal WordPress caching. Install a Redis object cache plugin to take advantage of it.

4. **Performance tuning:** MariaDB is configured with optimized settings for WordPress.

## Security Notes

- Change all default passwords in the `.env` file
- Consider using Docker secrets for production environments
- The current setup is intended for development; additional security measures needed for production

## Troubleshooting

### WordPress can't connect to database
- Ensure MariaDB container is running: `docker-compose ps`
- Check database credentials in `.env` file
- View MariaDB logs: `docker-compose logs mariadb`

### Permission issues
```bash
sudo chown -R www-data:www-data wp-content/
```

### Reset everything
```bash
docker-compose down -v
docker-compose up -d
```

This will remove all volumes and start fresh.
