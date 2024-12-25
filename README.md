# A simple MERN stack application 

This README outlines the steps to set up a simple MERN (MongoDB, Express, React, Node.js) stack application using Docker. The application consists of a frontend built with React and a backend built with Node.js and Express, connected to a MongoDB database.

### Architecture Diagram

```
+-------------------+       +-------------------+
|                   |       |                   |
|   React Frontend  | <--> |   Node.js Backend  |
|                   |       |                   |
+-------------------+       +-------------------+
                               |
                               |
                        +-------------------+
                        |                   |
                        |   MongoDB Database |
                        |                   |
                        +-------------------+
```

### Create a network for the docker containers

To allow the containers to communicate with each other, create a Docker network:

```sh
docker network create demo
```

### Build the client 

Navigate to the frontend directory and build the Docker image for the React application:

```sh
cd mern/frontend
docker build -t mern-frontend .
```

### Run the client

Run the frontend container, mapping port 5173 on your host to port 5173 in the container:

```sh
docker run --name=frontend --network=demo -d -p 5173:5173 mern-frontend
```

### Verify the client is running

Open your browser and type `http://localhost:5173` to access the frontend application.

### Run the mongodb container

Start the MongoDB container, ensuring it is connected to the same network:

```sh
docker run --network=demo --name mongodb -d -p 27017:27017 -v ~/opt/data:/data/db mongodb:latest
```

### Build the server

Navigate to the backend directory and build the Docker image for the Node.js application:

```sh
cd mern/backend
docker build -t mern-backend .
```

### Run the server

Run the backend container, mapping port 5050 on your host to port 5050 in the container:

```sh
docker run --name=backend --network=demo -d -p 5050:5050 mern-backend
```

## Using Docker Compose

To simplify the process of managing multiple containers, you can use Docker Compose. Run the following command to start all services defined in your `docker-compose.yml` file:

```sh
docker compose up -d
```