# Docker Lab - Todo Application

This repository contains a comprehensive Docker lab demonstrating containerization concepts using a Node.js Todo application. This lab covers Docker fundamentals including building images, running containers, networking, volumes, and Docker Compose.

## 📋 Table of Contents

- [Application Overview](#application-overview)
- [Prerequisites](#prerequisites)
- [Lab Objectives](#lab-objectives)
- [Getting Started](#getting-started)
- [Lab Exercises](#lab-exercises)
- [Screenshots Documentation](#screenshots-documentation)
- [Docker Compose Setup](#docker-compose-setup)
- [Testing the Application](#testing-the-application)
- [Cleanup](#cleanup)
- [Troubleshooting](#troubleshooting)

## 🚀 Application Overview

This is a simple Todo application built with:

- **Backend**: Node.js with Express.js framework
- **Database**: SQLite (development) / MySQL (production)
- **Frontend**: React.js with Bootstrap styling
- **Container**: Alpine Linux with Node.js LTS

### Application Features

- Add new todo items
- Mark items as complete/incomplete
- Delete todo items
- Persistent data storage
- Responsive web interface

## 🔧 Prerequisites

Before starting this lab, ensure you have:

- Docker Desktop installed and running
- Basic understanding of command line operations
- Text editor (VS Code recommended)
- Web browser for testing

## 🎯 Lab Objectives

By completing this lab, you will learn:

1. **Docker Fundamentals**

   - Creating Dockerfiles
   - Building Docker images
   - Running containers
   - Port mapping and networking

2. **Docker Networking**

   - Creating custom networks
   - Container-to-container communication
   - Network isolation

3. **Data Persistence**

   - Docker volumes
   - Bind mounts
   - Data persistence across container restarts

4. **Multi-Container Applications**

   - Docker Compose
   - Service orchestration
   - Environment variables

5. **Docker Hub Integration**
   - Tagging images
   - Pushing to registry
   - Pulling images from registry

## 🚀 Getting Started

### 1. Clone and Navigate to Project

```bash
cd getting-started-app
ls -la
```

### 2. Examine the Application Structure

```
Docker lab/
├── getting-started-app/        # Todo application source
│   ├── src/                   # Application source code
│   │   ├── index.js          # Main application entry point
│   │   ├── persistence/      # Database layer
│   │   ├── routes/           # API endpoints
│   │   └── static/           # Frontend assets (HTML, CSS, JS)
│   ├── spec/                 # Test files
│   ├── Dockerfile            # Container build instructions
│   ├── compose.yaml          # Docker Compose configuration
│   └── package.json          # Node.js dependencies
├── Screenshots/              # Lab documentation screenshots
└── README.md                # This documentation
```

## 🧪 Lab Exercises

### Exercise 1: Building Your First Docker Image

#### Step 1: Navigate to Application Directory

```bash
cd getting-started-app
```

#### Step 2: Examine the Dockerfile

```dockerfile
# syntax=docker/dockerfile:1
FROM node:lts-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

#### Step 3: Build the Docker Image

```bash
docker build -t getting-started-app .
```

**📸 Reference Screenshot**: `docker_build_console_output.png`

This screenshot shows the complete build process including:

- Layer creation and caching
- Package installation via yarn
- Final image creation
- Build completion confirmation

### Exercise 2: Running Your Container

#### Step 1: Run the Container

```bash
docker run -dp 127.0.0.1:3000:3000 getting-started-app
```

**📸 Reference Screenshot**: `docker_run_console_output.png`

#### Step 2: Verify Container is Running

```bash
docker ps
```

**📸 Reference Screenshot**: `docker_ps_for_running_containers.png`

This screenshot demonstrates:

- Active container listing
- Container ID and image information
- Port mappings (127.0.0.1:3000->3000/tcp)
- Container status and uptime

#### Step 3: Test the Application

Open your web browser and navigate to: `http://localhost:3000`

**📸 Reference Screenshot**: `app_running_in_browser.png`

This screenshot shows:

- Todo application interface
- Functional web application in browser
- Add/remove todo functionality working
- Responsive design elements

### Exercise 3: Docker Networking

#### Step 1: Create a Custom Network

```bash
docker network create todo-app
```

**📸 Reference Screenshot**: `docker_network_created.png`

This demonstrates:

- Network creation command execution
- Network ID generation
- Successful network establishment

#### Step 2: Run MySQL Container with Network

```bash
docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:8.0
```

### Exercise 4: Docker Volumes for Data Persistence

#### Step 1: Create a Named Volume

```bash
docker volume create todo-mysql-data
```

**📸 Reference Screenshot**: `docker_volume_created.png`

This screenshot shows:

- Volume creation process
- Volume name confirmation
- Successful volume establishment for data persistence

#### Step 2: Run Application with Network and Volume

```bash
docker run -dp 127.0.0.1:3000:3000 \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  getting-started-app
```

### Exercise 5: Docker Hub Integration

#### Step 1: Tag Your Image

```bash
docker tag getting-started-app YOUR_USERNAME/getting-started-app
```

#### Step 2: Push to Docker Hub

```bash
docker push YOUR_USERNAME/getting-started-app
```

**📸 Reference Screenshot**: `image_pushed_to_dockerhub.png`

This screenshot demonstrates:

- Image push process to Docker Hub
- Layer uploads and progress
- Successful registry publication

#### Step 3: Pull Image on Different Machine

```bash
docker pull YOUR_USERNAME/getting-started-app
```

**📸 Reference Screenshot**: `pulled_image_on_docker_lab.png`

This shows:

- Image pull from Docker Hub registry
- Layer downloads and verification
- Successful image retrieval

### Exercise 6: Docker Compose Multi-Container Setup

#### Step 1: Examine Docker Compose Configuration

The `getting-started-app/compose.yaml` file defines:

- **App Service**: Node.js application container
- **MySQL Service**: Database container
- **Volume**: Persistent data storage
- **Environment Variables**: Database connection settings

#### Step 2: Run with Docker Compose

```bash
cd getting-started-app
docker compose up -d
```

#### Step 3: View Logs

```bash
docker compose logs -f
```

#### Step 4: Verify Services

```bash
docker compose ps
```

## 📸 Screenshots Documentation

The lab includes comprehensive visual documentation:

| Screenshot                             | Description                               |
| -------------------------------------- | ----------------------------------------- |
| `docker_lab_instance.png`              | Lab environment setup and initial state   |
| `docker_build_console_output.png`      | Complete Docker build process and output  |
| `docker_run_console_output.png`        | Container startup and runtime information |
| `docker_ps_for_running_containers.png` | Active containers listing and status      |
| `app_running_in_browser.png`           | Functional web application in browser     |
| `docker_network_created.png`           | Custom network creation and configuration |
| `docker_volume_created.png`            | Volume creation for data persistence      |
| `image_pushed_to_dockerhub.png`        | Registry push process and confirmation    |
| `pulled_image_on_docker_lab.png`       | Image pull from Docker Hub registry       |
| `stop_and_remove_containers.png`       | Container cleanup and removal process     |

## 🐳 Docker Compose Setup

### Development Environment

The `getting-started-app/compose.yaml` provides a complete development environment:

```yaml
services:
  app:
    image: node:lts-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

### Quick Start Commands

```bash
# Navigate to app directory
cd getting-started-app

# Start all services
docker compose up -d

# View service logs
docker compose logs -f app

# Stop all services
docker compose down

# Remove volumes (careful - deletes data!)
docker compose down -v
```

## 🧪 Testing the Application

### Functional Testing

1. **Add Todo Items**: Create several todo items through the web interface
2. **Toggle Completion**: Mark items as complete/incomplete
3. **Delete Items**: Remove completed items
4. **Data Persistence**: Restart containers and verify data persists

### API Testing

The application provides RESTful endpoints:

```bash
# Get all items
curl http://localhost:3000/items

# Add new item
curl -X POST http://localhost:3000/items \
  -H "Content-Type: application/json" \
  -d '{"name": "Test item", "completed": false}'

# Update item
curl -X PUT http://localhost:3000/items/1 \
  -H "Content-Type: application/json" \
  -d '{"name": "Updated item", "completed": true}'

# Delete item
curl -X DELETE http://localhost:3000/items/1
```

## 🧹 Cleanup

### Stop and Remove Containers

```bash
# Using Docker Compose (from getting-started-app directory)
docker compose down

# Manual cleanup
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
```

**📸 Reference Screenshot**: `stop_and_remove_containers.png`

This screenshot shows:

- Container stopping process
- Container removal commands
- System cleanup confirmation

### Remove Images and Volumes

```bash
# Remove images
docker rmi getting-started-app
docker rmi YOUR_USERNAME/getting-started-app

# Remove volumes (caution: deletes data)
docker volume rm todo-mysql-data

# Remove networks
docker network rm todo-app
```

## 🔧 Troubleshooting

### Common Issues and Solutions

#### Port Already in Use

```bash
# Check what's using port 3000
netstat -tulpn | grep :3000

# Kill process or use different port
docker run -dp 127.0.0.1:3001:3000 getting-started-app
```

#### Container Won't Start

```bash
# Check container logs
docker logs CONTAINER_ID

# Check if image exists
docker images | grep getting-started-app
```

#### Database Connection Issues

```bash
# Verify MySQL container is running
docker ps | grep mysql

# Check network connectivity
docker exec -it APP_CONTAINER ping mysql
```

#### Volume Mount Problems

```bash
# List volumes
docker volume ls

# Inspect volume
docker volume inspect todo-mysql-data
```

### Useful Debug Commands

```bash
# Execute shell in running container
docker exec -it CONTAINER_ID sh

# Inspect container configuration
docker inspect CONTAINER_ID

# Monitor container resource usage
docker stats

# View Docker system information
docker system df
```

## 🎓 Learning Resources

- [Official Docker Documentation](https://docs.docker.com/)
- [Docker Compose Reference](https://docs.docker.com/compose/)
- [Node.js Best Practices in Docker](https://github.com/nodejs/docker-node/blob/main/docs/BestPractices.md)
- [Docker Hub Registry](https://hub.docker.com/)

## 📝 Lab Summary

This lab provided hands-on experience with:

✅ **Docker Image Building** - Creating custom images with Dockerfile  
✅ **Container Management** - Running, stopping, and monitoring containers  
✅ **Networking** - Custom networks and container communication  
✅ **Data Persistence** - Volumes and bind mounts  
✅ **Multi-Container Apps** - Docker Compose orchestration  
✅ **Registry Operations** - Pushing and pulling from Docker Hub  
✅ **Production Deployment** - Best practices and configurations

## 🤝 Contributing

Feel free to submit issues, fork the repository, and create pull requests for improvements.

---

**Note**: This lab is part of the AmaliTech DevOps Upskilling program and follows Docker best practices for educational purposes.
