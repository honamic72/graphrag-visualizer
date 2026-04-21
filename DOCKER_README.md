# Docker Setup for GraphRAG Visualizer

This directory contains Docker configuration files for containerizing the GraphRAG Visualizer application.

## Files

- `Dockerfile` - Multi-stage Docker build file
- `docker-compose.yml` - Docker Compose configuration
- `nginx.conf` - Nginx server configuration
- `.env.example` - Example environment variables file
- `.dockerignore` - Files to exclude from Docker build

## Quick Start

### Build and Run with Docker Compose

```bash
# Copy the example environment file
cp .env.example .env

# Build and start the container
docker-compose up --build -d

# View logs
docker-compose logs -f

# Stop the container
docker-compose down
```

### Build with Docker

```bash
# Build the image
docker build -t graphrag-visualizer .

# Run the container
docker run -d -p 3000:80 --name graphrag-visualizer graphrag-visualizer
```

## Configuration

### Environment Variables

You can customize the build using environment variables in the `.env` file:

- `HOST_PORT` - Host port mapping (default: 3000)
- `REACT_APP_API_URL` - API URL for the React app (optional)
- `REACT_APP_VERSION` - Application version (default: 0.1.0)

### Build Arguments

The Dockerfile supports the following build arguments:

- `REACT_APP_API_URL` - Set API URL during build time
- `REACT_APP_VERSION` - Set application version during build time

Example:
```bash
docker build --build-arg REACT_APP_API_URL=https://api.example.com \
             --build-arg REACT_APP_VERSION=1.0.0 \
             -t graphrag-visualizer .
```

## Volumes

The docker-compose.yml mounts the following volumes:

- `./public/artifacts:/usr/share/nginx/html/artifacts:ro` - Read-only access to artifacts directory
- `nginx_logs:/var/log/nginx` - Persistent nginx logs

## Accessing the Application

Once the container is running, you can access the application at:

- http://localhost:3000 (or the port specified in HOST_PORT)

## Health Check

The container includes a health check endpoint:

- http://localhost:3000/health

## Stopping and Cleaning Up

```bash
# Stop containers
docker-compose down

# Remove volumes (including nginx logs)
docker-compose down -v

# Remove all containers, volumes, and images
docker-compose down -v --rmi all
```

## Production Considerations

For production deployment:

1. Set appropriate environment variables
2. Use a specific tag instead of 'latest'
3. Configure proper SSL/TLS termination
4. Set up log rotation for nginx logs
5. Consider using a reverse proxy like Traefik or Nginx Proxy Manager
