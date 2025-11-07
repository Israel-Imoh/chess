# Chess Game - Docker Deployment Guide

## 📋 Prerequisites

- Docker installed (version 20.10 or higher)
- Docker Compose installed (version 2.0 or higher)

## 📁 Project Structure

```
chess-game/
├── src/                    # Your React source code
├── public/                 # Public assets
├── package.json           # Dependencies
├── vite.config.js         # Vite configuration
├── Dockerfile             # Docker build instructions
├── docker-compose.yml     # Docker Compose configuration
├── nginx.conf             # Nginx server configuration
└── README.md              # This file
```

## 🚀 Quick Start

### Option 1: Using Docker Compose (Recommended)

1. **Build and start the container:**

   ```bash
   docker-compose up -d --build
   ```

2. **Access the application:**
   Open your browser and navigate to:

   ```
   http://localhost:3000
   ```

3. **Stop the container:**
   ```bash
   docker-compose down
   ```

### Option 2: Using Docker directly

1. **Build the Docker image:**

   ```bash
   docker build -t chess-game:latest .
   ```

2. **Run the container:**

   ```bash
   docker run -d -p 3000:80 --name chess-game chess-game:latest
   ```

3. **Access the application:**

   ```
   http://localhost:3000
   ```

4. **Stop and remove the container:**
   ```bash
   docker stop chess-game
   docker rm chess-game
   ```

## 🔧 Configuration

### Change Port

To run the application on a different port, modify the `docker-compose.yml`:

```yaml
ports:
  - "8080:80" # Change 8080 to your desired port
```

Or with Docker command:

```bash
docker run -d -p 8080:80 --name chess-game chess-game:latest
```

## 📊 Useful Docker Commands

```bash
# View running containers
docker ps

# View container logs
docker logs chess-game

# View logs in real-time
docker logs -f chess-game

# Rebuild without cache
docker-compose build --no-cache

# Remove all stopped containers and unused images
docker system prune -a
```

## 🏗️ Build Process

The Dockerfile uses a **multi-stage build**:

1. **Stage 1 (Build):**

   - Uses Node.js 18 Alpine image
   - Installs dependencies
   - Builds the Vite React application

2. **Stage 2 (Production):**
   - Uses Nginx Alpine image (lightweight)
   - Copies built files from Stage 1
   - Serves the static files

This approach results in a much smaller final image (~25MB vs ~500MB).

## 🌐 Production Deployment

For production deployment:

1. Update the `nginx.conf` with your domain name
2. Consider adding SSL/TLS certificates
3. Set up environment variables for API endpoints
4. Use a reverse proxy (like Nginx or Traefik) if needed

## 🐛 Troubleshooting

### Port already in use

```bash
# Check what's using the port
lsof -i :3000

# Use a different port in docker-compose.yml
```

### Build fails

```bash
# Clean Docker cache
docker builder prune

# Rebuild from scratch
docker-compose build --no-cache
```

### Cannot access the application

```bash
# Check if container is running
docker ps

# Check container logs
docker logs chess-game

# Verify port mapping
docker port chess-game
```

## 📦 Image Size

- **Development image:** ~500MB (with Node.js)
- **Production image:** ~25MB (Nginx + built files)

## 🔐 Security Notes

- The nginx.conf includes basic security headers
- Static files are served with appropriate caching
- No sensitive data should be in the Docker image
- For production, use environment variables for configuration

## 📝 Notes

- The application runs on port 80 inside the container
- Port 3000 on your host machine is mapped to port 80 in the container
- The build uses npm; if you use yarn, update the Dockerfile accordingly
- Logs are available via `docker logs chess-game`
