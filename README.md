# Microservices with Docker Compose

This project demonstrates a microservices architecture using Docker Compose with:

- **service_1**: Golang application running on port 8001
- **service_2**: Python Flask application running on port 8002  
- **nginx**: Custom nginx reverse proxy running on port 8080

## Architecture

```
Client -> Nginx (port 8080) -> service_1 (port 8001) [Golang]
                            -> service_2 (port 8002) [Python]
```

## Usage

### Start all services
```bash
docker-compose up -d
```

### Stop all services
```bash
docker-compose down
```

### View logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f service_1
docker-compose logs -f service_2
docker-compose logs -f nginx
```

### Rebuild services
```bash
docker-compose up -d --build
```

## API Endpoints

### Through Nginx Reverse Proxy (port 8080)

- **GET** `http://localhost:8080/` - Nginx status page
- **GET** `http://localhost:8080/health` - Nginx health check
- **GET** `http://localhost:8080/go/` - Golang service hello
- **GET** `http://localhost:8080/go/health` - Golang service health
- **GET** `http://localhost:8080/py/` - Python service hello  
- **GET** `http://localhost:8080/py/health` - Python service health
- **GET** `http://localhost:8080/py/data` - Python service data

### Direct Access (for testing)

- **Golang Service** (port 8001): `http://localhost:8001/`
- **Python Service** (port 8002): `http://localhost:8002/`

## Network Configuration

All services are connected via a custom bridge network called `bridge_network`, enabling service-to-service communication using container names as hostnames.

## Docker Images

All services use custom Dockerfiles:
- **service_1**: Multi-stage Golang build for optimized image size
- **service_2**: Python Flask application with pip dependencies
- **nginx**: Custom nginx image with embedded configuration

## Testing

After starting the services, test the setup:

```bash
# Test nginx reverse proxy
curl http://localhost:8080/

# Test golang service through nginx
curl http://localhost:8080/go/ping
curl http://localhost:8080/go/hello

# Test python service through nginx  
curl http://localhost:8080/py/ping
curl http://localhost:8080/py/hello

## Development

### Building individual services
```bash
# Build specific service
docker-compose build service_1
docker-compose build service_2
docker-compose build nginx

# Build all services
docker-compose build
```

### Scaling services
```bash
# Scale golang service to 3 instances
docker-compose up -d --scale service_1=3

# Scale python service to 2 instances
docker-compose up -d --scale service_2=2
```