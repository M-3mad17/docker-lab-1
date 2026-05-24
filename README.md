# docker-lab-1

---

### What is the difference between: CMD & ENTRYPOINT
* **CMD:** Sets a default command that can be easily overwritten from the command line using `docker run [IMAGE] [COMMAND]`.
* **ENTRYPOINT:** Sets a command that will always be executed when the container starts. Arguments passed via `docker run` are appended to the ENTRYPOINT.

---

### What is the difference between: COPY & ADD
* **COPY:** Copies local files or directories from the host machine to the container.
* **ADD:** Does the same as COPY but has extra features, such as downloading files from URLs and automatically extracting compressed files (.tar, .gzip, etc.).

---

### Problem 1: Run the container hello-world
```bash
docker run --name my-hello hello-world

________________________
Problem 1: Check the container status
docker ps -a
__________________
Problem 1: Start the stopped container
docker start -a my-hello
___________________________
Problem 1: Remove the container
docker rm my-hello
_________________________
Problem 1: Remove the image
docker rmi hello-world
__________________________
Problem 2: Run container centos or ubuntu in an interactive mode
docker run -it --name my-ubuntu ubuntu bash
__________________________
Problem 2: Run the following command in the container "echo docker "
echo docker
_________________________
Problem 2: Open a bash shell in the container and touch a file named hello-docker
touch hello-docker
__________________________
Problem 2: Stop the container and remove it. Write your comment about the file hello-docker
# Exit the container first by typing 'exit', then run:
docker stop my-ubuntu
docker rm my-ubuntu

____________________________
Problem 2: Remove all stopped containers
docker container prune -f

___________________________
Problem 3: Deploy a MySQL database called app-database. Use the mysql latest image, and use the -e flag to set MYSQL_ROOT_PASSWORD to P4sSw0rdO!. The container should run in the background.

docker run -d --name app-database -e MYSQL_ROOT_PASSWORD=P4sSw0rdO! -p 3306:3306 mysql:latest
__________________________
Problem 4: Run the image Nginx
docker run -d --name my-nginx -p 8080:80 nginx
----------------------------____
Problem 4: Add html static files to the container and make sure they are accessible
docker cp index.html my-nginx:/usr/share/nginx/html/
_______________________________
Problem 4: Commit the container with image name IMAGE_NAME
docker commit my-nginx my-custom-nginx-image
_________________________________
Problem 5: Create a python simple app
Create a file named app.py and paste this code:

print("Hello from Python Docker App!")

________________________________
Problem 5: Create a dockerfile to containerize the python app & (Bonus) create a dockerfile for the same app in smaller size using multi staging

# Stage 1: Build stage
FROM python:3.9-slim AS builder
WORKDIR /app
COPY app.py .

# Stage 2: Final production stage (Uses smaller alpine image)
FROM python:3.9-alpine
WORKDIR /app
COPY --from=builder /app /app
CMD ["python", "app.py"]


_________________________________
Problem 5: Build the image and test it
# Build the image
docker build -t your_dockerhub_username/python-simple-app:latest .

# Test the image
docker run --rm your_dockerhub_username/python-simple-app:latest

____________________________________
Problem 5: Push the created image into your docker hub repo

docker login
docker push your_dockerhub_username/python-simple-app:latest
