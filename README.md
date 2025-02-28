# MongoDB Docker Setup

This guide demonstrates how to set up MongoDB in a Docker container using Docker Compose and connect to it using MongoDB Compass.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [MongoDB Compass](https://www.mongodb.com/products/compass) (GUI client for MongoDB)

## Setting Up MongoDB with Docker Compose

### 1. Create Project Directory

```bash
mkdir mongodb-docker
cd mongodb-docker
```

### 2. Create Docker Compose Configuration

Create a file named `docker-compose.yml` with the following content:

```yaml
version: '3.8'

services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    restart: always
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
      - MONGO_INITDB_DATABASE=mydatabase
    ports:
      - 27017:27017
    volumes:
      - mongodb_data:/data/db
    networks:
      - mongo_network

networks:
  mongo_network:
    driver: bridge

volumes:
  mongodb_data:
    driver: local
```

### 3. Start MongoDB Container

Run the following command to start MongoDB in detached mode:

```bash
docker compose up -d
```

### 4. Verify Container Status

Ensure the container is running correctly:

```bash
docker ps
```

You should see output similar to:

```
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                      NAMES
a1b2c3d4e5f6   mongo:latest   "docker-entrypoint.s…"   1 minute ago    Up 1 minute    0.0.0.0:27017->27017/tcp   mongodb
```

## Connecting with MongoDB Compass

### 1. Install MongoDB Compass

Download and install MongoDB Compass from the [official website](https://www.mongodb.com/products/compass).

### 2. Connect to MongoDB

1. Open MongoDB Compass
2. In the connection string field, enter:
   ```
   mongodb://admin:password@localhost:27017/mydatabase?authSource=admin
   ```
3. Click the "Connect" button

### 3. Connection String Breakdown

- `mongodb://` - Protocol
- `admin:password` - Username and password defined in docker-compose.yml
- `localhost:27017` - Host and port
- `mydatabase` - Default database name
- `authSource=admin` - Database containing the user credentials

## Managing MongoDB

### Basic Docker Commands

- Stop the container:
  ```bash
  docker compose down
  ```

- Stop and remove volumes (deletes all data):
  ```bash
  docker compose down -v
  ```

- View container logs:
  ```bash
  docker logs mongodb
  ```

- Access MongoDB shell in the container:
  ```bash
  docker exec -it mongodb mongosh -u admin -p password
  ```

### MongoDB Compass Features

Once connected with MongoDB Compass, you can:

- Create and manage databases
- Create and manage collections
- Insert, update, and delete documents
- Run queries with the query builder
- Analyze data with visualizations
- Import and export data

## Customization Options

### Modifying MongoDB Configuration

To customize MongoDB settings, you can:

1. Create a custom `mongod.conf` file
2. Mount it to the container by adding to the volumes section:

```yaml
volumes:
  - ./mongod.conf:/etc/mongod.conf
  - mongodb_data:/data/db
```

3. Add a command to use this configuration:

```yaml
command: mongod --config /etc/mongod.conf
```

### Initialization Scripts

To run scripts when the container first starts:

1. Create a directory for scripts:
   ```bash
   mkdir -p init-scripts
   ```

2. Add JavaScript or shell scripts to this directory
3. Mount the directory in docker-compose.yml:

```yaml
volumes:
  - ./init-scripts:/docker-entrypoint-initdb.d/
  - mongodb_data:/data/db
```

## Troubleshooting

### Common Issues

1. **Can't connect to MongoDB**
   - Check if the container is running: `docker ps`
   - Verify port mapping: `docker port mongodb`
   - Test connection locally: `docker exec -it mongodb mongosh -u admin -p password`

2. **Authentication failed**
   - Verify the credentials in docker-compose.yml
   - Check if the authSource is correctly specified in the connection string

3. **Data persistence issues**
   - Ensure the volume is properly configured
   - Check volume permissions: `docker volume inspect mongodb_data`

## Security Notes

- Change the default username and password in production environments
- Consider using Docker secrets or environment files for credentials
- Limit network exposure by binding to specific network interfaces
- Implement MongoDB authentication and authorization mechanisms

## Further Reading

- [MongoDB Documentation](https://docs.mongodb.com/)
- [Docker Documentation](https://docs.docker.com/)
- [MongoDB Compass Documentation](https://docs.mongodb.com/compass/current/)
