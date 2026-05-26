Docker Configuration for Flask App

This document explains the configuration of the Dockerfile used to containerize the Flask "Hello World" application.

Dockerfile Breakdown

The Dockerfile follows these steps to ensure a lightweight and functional environment:

1. Base Image: FROM python:3.14.3-slim
Uses a minimal version of Python 3.14.3 to keep the image size small.
2. Working Directory: WORKDIR /app
Sets the internal path where all commands will be executed.
3. Dependency Installation:
Runs pip install with --no-cache-dir to save space.
4. Copy Source Code: COPY helloworld.py .
Moves your local application into the container.
5. Port Exposure: EXPOSE 8080
Informs Docker that the container listens on port 8080 at runtime.
6. Startup Command: CMD \["python", "app.py"]
Tells the container to start the Flask server immediately upon launch.



Usage Instructions

Build the Image

1. To create the Docker image locally, use the following command:
bash
docker build -t my-flask-app .

Use code with caution.
-t my-flask-app: Tags the image with a friendly name.
.: Tells Docker to look for the Dockerfile in the current folder.

2. Run the Container
To start a container based on the image:
bash
docker run -d -p 8080:8080 --name flask-app my-flask-app
Use code with caution.
-d: Runs the container in detached mode (in the background).
-p 8080:8080: Maps your computer's port 8080 to the container's port 8080.
--name flask-app: Gives your running container a specific name.

Inspection \& Debugging

Check logs: docker logs flask-app
Enter the container (Shell): docker exec -it flask-app sh
Check image size: docker images my-flask-app

