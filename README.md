# MongoDB Docker Setup

This guide demonstrates how to run MongoDB in a Docker container using Docker Compose and connect to it using MongoDB Compass.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [MongoDB Compass](https://www.mongodb.com/products/compass) (optional, for GUI access)

## Quick Start

### 1. Clone the Repository

```bash
git clone [[repository-url](https://github.com/DanielLanghann/mongo_db.git)]
cd [path_to_repository]
```

### 2. Start MongoDB Container

```bash
docker compose up -d
```

This command starts MongoDB in detached mode using the provided docker-compose.yml configuration.

### 3. Verify Container Status

```bash
docker ps
```

You should see the MongoDB container running.

## Connecting to MongoDB

### Using MongoDB Compass

1. Download and install MongoDB Compass from the [official website](https://www.mongodb.com/products/compass) if you haven't already

2. Open MongoDB Compass

3. On the connection screen, you can connect using either:

   **Option 1: Connection string**
   - Enter the following connection string:
     ```
     mongodb://admin:password@localhost:27017/mydatabase?authSource=admin
     ```

   **Option 2: Form fields**
   - Hostname: `localhost`
   - Port: `27017`
   - Authentication: Username/Password
   - Username: `admin`
   - Password: `password`
   - Authentication Database: `admin`
   - Database: `mydatabase`

4. Click "Connect"

5. Once connected, you can:
   - Browse the database structure
   - Create, view and modify collections
   - Execute queries
   - Import and export data

### Using Command Line

```bash
docker exec -it mongodb mongosh -u admin -p password
```

## Stopping the Container

```bash
docker compose down
```

To remove volumes and delete all data:

```bash
docker compose down -v
```

## Troubleshooting

If you can't connect to MongoDB:
- Check if the container is running: `docker ps`
- View container logs: `docker logs mongodb`
