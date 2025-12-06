---
sidebar_position: 3
sidebar_label: Practice Time
---

# Practice Time

## Prerequisites

1. Docker installed and running on your machine. Verify by running `docker --version`
   in your terminal.
2. Download Node.js from [nodejs.org](https://nodejs.org/en/download). Verify installation
   by running `node -v` and `npm -v` in your terminal.
3. Download Python from [python.org](https://www.python.org/downloads/). Verify installation
   by running `python --version` and `pip --version` in your terminal.

## Exercise 1: Containerizing Backend Node.js App

In this exercise, you'll containerize a simple Node.js backend application using
Docker.

### Step 1: Set up the Node.js Application

1. Create a new directory for your project and navigate into it

   ```sh
   mkdir node-app
   cd node-app
   ```

2. Initialize a new Node.js project

   ```sh
    npm init -y
   ```

3. Install Express.js (framework for building web applications)

   ```sh
   npm install express
   ```

4. Create an `index.js` file with the following content

   ```js
   const express = require("express");

   const app = express();
   const port = process.env.PORT || 4000;

   app.get("/", (req, res) => {
     res.send("Hello from Node.js!");
   });

   app.listen(port, () => {
     console.log(`Server listening at http://localhost:${port}`);
   });
   ```

5. Test the application locally

   ```sh
   node index.js
   ```

6. Open your browser and navigate to `http://localhost:4000`. You should see
   "Hello from Node.js!" displayed.

7. Stop the server by pressing `Ctrl + C` in your terminal.

8. Create a `Dockerfile` in the root of your project with the following content

   ```Dockerfile
    # Use an official Node.js runtime as a parent image
    FROM node:24-alpine

    # Set the working directory in the container
    WORKDIR /app

    # Copy package.json and package-lock.json
    COPY package*.json ./

    # Install dependencies
    RUN npm install

    # Copy the rest of the application code by ignoring files in .dockerignore
    COPY . .

    # Expose the port the app runs on
    EXPOSE 4000

    # Define the command to run the app
    CMD ["node", "index.js"]
   ```

9. Create a `.dockerignore` file to exclude unnecessary files from the Docker image

   ```plaintext
   node_modules
   Dockerfile
   .dockerignore
   ```

### Step 2: Build the Docker Image

1. Open your terminal and navigate to the project directory.
2. Run the following command to build the Docker image

   ```sh
   docker build -t node-app .
   ```

### Step 3: Run the Docker Container

1. After the image is built, run the container using the following command

   ```sh
   docker run -p 5000:4000 node-app
   ```

2. Open your web browser and navigate to `http://localhost:5000`.
   You should see "Hello from Node.js!" displayed.

3. To stop the container, go back to your terminal and press `Ctrl + C`.

4. Congratulations! You've successfully containerized a Node.js backend
   application using Docker.

---

## Exercise 2: Containerizing Backend Python App

In this exercise, you'll containerize a simple Python FastAPI backend application
using Docker.

### Step 1: Set up the FastAPI Application

1. Create a new directory for your project and navigate into it

   ```sh
   mkdir python-app
   cd python-app
   ```

2. Create and activate a virtual environment (optional but recommended)

   ```sh
   python -m venv .venv

   .venv\Scripts\activate
   ```

3. Install FastAPI and Uvicorn

   ```sh
   pip install "fastapi[standard]"
   ```

4. Create a `main.py` file with the following content

   ```python
    from fastapi import FastAPI

    app = FastAPI()


    @app.get("/")
    def read_root():
        return {
            "message": "Hello World from FastAPI!"
        }
   ```

5. Test the application locally

   ```sh
   fastapi run main.py
   ```

6. Open your browser and navigate to `http://localhost:8000`. You should see
   `{"message": "Hello from FastAPI!"}` displayed.

7. Stop the server by pressing `Ctrl + C` in your terminal.

8. Create a `Dockerfile` in the root of your project with the following content

   ```Dockerfile
    # Use an official Python runtime as a parent image
    FROM python:3.12-slim

    # Set the working directory in the container
    WORKDIR /app

    # Copy requirements.txt
    COPY requirements.txt ./

    # Install dependencies
    RUN pip install --no-cache-dir -r requirements.txt

    # Copy the rest of the application code by ignoring files in .dockerignore
    COPY . .

    # Expose the port the app runs on
    EXPOSE 8000

    # Define the command to run the app
    CMD ["fastapi", "run", "main.py" , "--host", "0.0.0.0", "--port", "8000"]
   ```

9. Create a `requirements.txt` file to list the dependencies

   ```plaintext
   fastapi[standard]
   ```

10. Create a `.dockerignore` file to exclude unnecessary files from the Docker image

    ```plaintext
    __pycache__
    *.pyc
    venv
    Dockerfile
    .dockerignore
    ```

### Step 2: Build the Python FastAPI Docker Image

1. Open your terminal and navigate to the project directory.
2. Run the following command to build the Docker image

   ```sh
   docker build -t fastapi-app .
   ```

### Step 3: Run the Python FastAPI Docker Container

1. After the image is built, run the container using the following command

   ```sh
   docker run -p 8000:8000 fastapi-app
   ```

2. Open your web browser and navigate to `http://localhost:8000`.
   You should see `{"message": "Hello from FastAPI!"}` displayed.

3. To stop the container, go back to your terminal and press `Ctrl + C`.

4. Congratulations! You've successfully containerized a Python FastAPI
   backend application using Docker.

---

## Exercise 3: Containerizing Frontend React App

In this exercise, you'll containerize a simple React frontend application using
Docker.

### Step 1: Set up the React Application

1. Create a new application using Vite

   ```sh
   npm create vite@latest my-react-app -- --template react
   ```

2. Navigate into the project directory

   ```sh
   cd my-react-app
   ```

3. Install dependencies

   ```sh
    npm install
   ```

4. Test the application locally by running the development server

   ```sh
   npm run dev
   ```

5. Open your browser and navigate to the URL provided in the terminal (usually `http://localhost:5173`).
   You should see the default Vite React application running.

6. Stop the server by pressing `Ctrl + C` in your terminal.

7. Create a `Dockerfile` in the root of your project with the following content
   (Introducing Multi-stage builds)

   ```Dockerfile
    # Stage 1: Build the React application
    FROM node:24-alpine AS build

    # Set the working directory in the container
    WORKDIR /app

    # Copy package.json and package-lock.json
    COPY package*.json ./

    # Install dependencies
    RUN npm install

    # Copy the rest of the application code by ignoring files in .dockerignore
    COPY . .

    # Build the React application for production
    RUN npm run build

    # Stage 2: Serve the built application using a lightweight web server
    FROM nginx:alpine

    # Copy the built files from the previous stage to Nginx's html directory
    COPY --from=build /app/dist /usr/share/nginx/html

    # Expose port 80
    EXPOSE 80

    # Start Nginx server
    CMD ["nginx", "-g", "daemon off;"]
   ```

8. Create a `.dockerignore` file to exclude unnecessary files from the Docker image

   ```plaintext
    node_modules
    build
    Dockerfile
    .dockerignore
   ```

:::tip

1. Use multi-stage builds in your Dockerfile to optimize the image size by
   separating the build environment from the runtime environment.
2. With multi-stage builds, you use multiple FROM statements in your Dockerfile.
   Each `FROM` instruction can use a different base, and each of them begins
   a new stage of the build.
3. You can selectively copy artifacts from one stage to another, leaving behind
   everything you don't want in the final image.
4. Learn more about multi-stage builds from the official Docker documentation:
   [Docker Multi-Stage Builds](https://docs.docker.com/develop/develop-images/multistage-build/)

:::

### Step 2: Build the React Docker Image

1. Open your terminal and navigate to the project directory.
2. Run the following command to build the Docker image

   ```sh
   docker build -t my-react-app .
   ```

### Step 3: Run the React Docker Container

1. After the image is built, run the container using the following command

   ```sh
   docker run -p 6001:80 my-react-app
   ```

2. Open your web browser and navigate to `http://localhost:6001`.
3. You should see the default Vite React application running.
4. To stop the container, go back to your terminal and press `Ctrl + C`.
5. Congratulations! You've successfully containerized a React frontend
   application using Docker.
