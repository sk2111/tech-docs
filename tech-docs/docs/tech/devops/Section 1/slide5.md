---
sidebar_position: 5
sidebar_label: Docker Compose
---

# Docker Compose

Imagine you have a web application that consists of multiple services, such as a
frontend, backend, and a database. Managing each of these services individually,
spin up & down can be complex and error-prone.

Docker Compose is a tool for defining and running multi-container applications.
It is the key to unlocking a streamlined and efficient development and deployment
experience

## Why Use Docker Compose?

1. **Simplified Configuration**: Define all your services, networks, and volumes
   in a single `docker-compose.yml` file.
2. **Multi-Container Applications**: Easily manage applications that require
   multiple services (e.g., web server, database, cache).
3. **Isolation**: Each service runs in its own container, ensuring that
   dependencies do not conflict.

Lets see how Docker Compose works for a simple web application.

![Docker Compose Overview](./assets/docker_compose_overview.png)

## Basic Concepts

1. **Service**: A service is a containerized application defined in the
   `docker-compose.yml` file.
2. **Network**: Docker network for your
   services to communicate.
3. **Volume**: Persistent storage that can be shared between services.
4. **Project**: A project is a collection of services defined in a
   `docker-compose.yml` file.

## Exercise: Creating a Docker Compose File

1. Create a new `docker-compose.yml` file in your `directory parallel to the
frontend & backend directories`.

2. Write the `docker-compose.yml` file with the following content

   ```yaml
   version: "3.9"

   name: my-web-app

   services:
     frontend:
       build: ./my-react-app
       container_name: frontend
       restart: always
       ports:
         - "6001:80"
       depends_on:
         - backend
       networks:
         - my-custom-network

     backend:
       build: ./node-app
       container_name: backend
       restart: always
       ports:
         - "5000:4000"
       depends_on:
         - database
       networks:
         - my-custom-network

     database:
       image: postgres:16-alpine
       container_name: database
       restart: always
       environment:
         POSTGRES_USER: user
         POSTGRES_PASSWORD: password
         POSTGRES_DB: postgres
       ports:
         - "5432:5432"
       volumes:
         - pg-data:/var/lib/postgresql/data
       networks:
         - my-custom-network

   volumes:
     pg-data:

   networks:
     my-custom-network:
       driver: bridge
   ```

   :::warning
   This is just an example. The frontend, backend & database are not wired up
   :::

3. Services restart possible values are:

   1. `no`: Do not automatically restart the container. (default)
   2. `always`: Always restart the container if it stops.
   3. `on-failure`: Restart the container only if it exits with a non-zero status.
   4. `unless-stopped`: Always restart the container unless it is explicitly stopped.

4. Start the services using Docker Compose:

   ```sh
   docker compose up
   ```

5. Open your web browser and navigate to `http://localhost:6001` to see the frontend

6. Stop the services:

   ```sh
   docker compose down
   ```

:::tip[Production Tip]

:::

1. Use environment variables to manage sensitive data like passwords.
2. Keep your `docker-compose.yml` file organized and well-documented.
3. Use named volumes for persistent data storage.
4. Leverage Docker Compose for local development and testing.

## Summary

1. Docker Compose simplifies the management of multi-container applications.
2. It allows you to define services, networks, and volumes in a single file.
3. Using Docker Compose can streamline your development and deployment workflows.
4. Follow best practices to ensure a smooth experience with Docker Compose.

:::tip[Learn More]
Learn more about Docker Compose in the [official documentation](https://docs.docker.com/compose/).
:::
