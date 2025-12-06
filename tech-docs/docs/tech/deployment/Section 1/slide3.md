---
sidebar_position: 3
sidebar_label: Practice Time
---

# Practice Time

## Exercise 1: Containerizing Backend Node.js App

In this exercise, you'll containerize a simple Node.js backend application using
Docker.

### Prerequisites

1. Docker installed and running on your machine. Verify by running `docker --version`
   in your terminal.
2. Download Node.js from [nodejs.org](https://nodejs.org/en/download). Verify installation
   by running `node -v` and `npm -v` in your terminal.

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
