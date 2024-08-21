# JavaScript App using Docker

This app showcases a simple user profile application set up using:

- `index.html` with pure JavaScript and CSS styles
- Node.js backend with the Express module
- MongoDB for data storage

All components are Docker-based.

## Getting Started with Docker

### Prerequisites

Before you begin, ensure you have the following installed:

- Docker: [Install Docker](https://docs.docker.com/get-docker/)
- Docker Compose: [Install Docker Compose](https://docs.docker.com/compose/install/)
- Node.js and npm: [Install Node.js](https://nodejs.org/)

### To Start the Application Using Docker

#### Step 1: Create Docker Network

Create a Docker network to allow containers to communicate with each other.

```sh
docker network create mongo-network
```

#### Step 2: Start MongoDB

Run a MongoDB container on the created network.

```sh
docker run -d -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  --name mongodb \
  --net mongo-network \
  mongo
```

- `-d`: Run the container in detached mode.
- `-p 27017:27017`: Map port 27017 of the container to port 27017 of the host.
- `-e MONGO_INITDB_ROOT_USERNAME=admin`: Set the MongoDB root username.
- `-e MONGO_INITDB_ROOT_PASSWORD=password`: Set the MongoDB root password.
- `--name mongodb`: Name the container `mongodb`.
- `--net mongo-network`: Connect the container to the `mongo-network` network.
- `mongo`: Use the official MongoDB image.

#### Step 3: Start Mongo-Express

Run a Mongo-Express container to provide a web-based interface for MongoDB.

```sh
docker run -d -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  --net mongo-network \
  --name mongo-express \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  mongo-express
```

- `-d`: Run the container in detached mode.
- `-p 8081:8081`: Map port 8081 of the container to port 8081 of the host.
- `-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin`: Set the MongoDB admin username for Mongo-Express.
- `-e ME_CONFIG_MONGODB_ADMINPASSWORD=password`: Set the MongoDB admin password for Mongo-Express.
- `--net mongo-network`: Connect the container to the `mongo-network` network.
- `--name mongo-express`: Name the container `mongo-express`.
- `-e ME_CONFIG_MONGODB_SERVER=mongodb`: Set the MongoDB server address to `mongodb`.
- `mongo-express`: Use the official Mongo-Express image.

**Note:** Creating a Docker network is optional. You can start both containers in the default network. In this case, omit the `--net` flag in the `docker run` command.

#### Step 4: Open Mongo-Express in Browser

Open [http://localhost:8081](http://localhost:8081) in your web browser to access the Mongo-Express interface.

#### Step 5: Create `user-account` Database and `users` Collection in Mongo-Express

- In the Mongo-Express interface, create a new database named `user-account`.
- Within the `user-account` database, create a new collection named `users`.

#### Step 6: Start Your Node.js Application Locally

Navigate to the app directory of your project and install dependencies.

```sh
npm install 
```

Start the Node.js server.

```sh
node server.js
```

#### Step 7: Access the Node.js Application UI in Browser

Open [http://localhost:3000](http://localhost:3000) in your web browser to access the Node.js application UI.

### To Start the Application Using Docker Compose

#### Step 1: Start MongoDB and Mongo-Express

Use Docker Compose to start both MongoDB and Mongo-Express containers.

```sh
docker-compose -f docker-compose.yaml up
```

You can access Mongo-Express at [http://localhost:8080](http://localhost:8080) from your browser.

#### Step 2: Create a New Database "my-db" in Mongo-Express UI

In the Mongo-Express interface, create a new database named `my-db`.

#### Step 3: Create a New Collection "users" in the Database "my-db"

Within the `my-db` database, create a new collection named `users`.

#### Step 4: Start Node Server

Navigate to the app directory and install dependencies.

```sh
npm install
```

Start the Node.js server.

```sh
node server.js
```

#### Step 5: Access the Node.js Application from Browser

Open [http://localhost:3000](http://localhost:3000) in your web browser to access the Node.js application UI.

## Building a Docker Image from the Application

To build a Docker image for the application, run the following command in the project root directory:

```sh
docker build -t my-app:1.0 .
```

- `-t my-app:1.0`: Tag the image as `my-app` with version `1.0`.
- The dot (`.`) at the end of the command denotes the location of the Dockerfile.


