# 📌 Important Docker Commands and Concepts 🐳

## 🚀 Docker Run Command
- `docker run <image>`: Runs a container from an image.
- `docker run -it ubuntu bash` : allowing you to access the container’s terminal 
- `-i` (interactive mode) 🖥️: Keeps STDIN open so you can provide input to the container.
- `-t` (pseudo-TTY) 📟: Allocates a terminal session, making it more user-friendly.
- `--name` webapp 
- If the image is not present, Docker pulls it from **Docker Hub**.
- Subsequent executions reuse the same image.

## 📜 Listing Containers
- `docker ps`: Lists all **running** containers with details like ID, image, status, and name.
- `docker ps -a`: Lists **all containers**, including stopped ones.

## ⏹️ Stopping and Removing Containers
- `docker stop <container_id/name>`: Stops a running container. prints the container after succesful.
- `docker start <container_id/name>`: Starts a stopped container.
- `docker rm <container_id/name>`: Removes a **stopped** container permanently.

## 📦 Managing Images
- `docker images`: Lists all available images on the host.
- `docker rmi <image_name:TAG>`: Removes an image (only if no containers are using it).
- `docker pull <image_name:tag>`: Downloads an image without running it.

## 🏗️ Running and Exiting Containers
- `docker run ubuntu`: Runs an Ubuntu container, but it **exits immediately** because it has no process running, it is just an os, no application is running.
- Containers **only run while a process is alive**.
- Example: Running a **sleep** process keeps it active:
  ```sh
  docker run ubuntu sleep 5
  ```
  - This runs a sleep command inside Ubuntu for **5 seconds**, then the container stops.

## 🎯 Executing Commands in Running Containers
- `docker exec <container_id/name> <command>`: Runs a command inside a running container.
- Example: Viewing **/etc/hosts** file inside a container:
  ```sh
  docker exec <container_id> cat /etc/hosts
  ```

## 🌐 Running Web Applications
- Running a simple web application:
  ```sh
  docker run cloud/simple-web-app
  ```
- **By default, runs in attached mode**, showing logs but not allowing other commands.
- **Press `Ctrl + C`** to stop the container.

## 🔄 Running Containers in Detached Mode
- `docker run -d <image>`: Runs a container in the **background**.
- `docker run -d --name webapp nginx:1.14-alpine` : give name using name
- View running containers:
  ```sh
  docker ps
  ```
- Attach back to a running container:
  ```sh
  docker attach <container_id>
  ```
- You can use **partial container IDs** if they are unique.

## 🔥 Summary
- Docker containers run isolated processes.
- Stopped containers **do not consume resources** but take up space.
- Use `docker ps`, `docker images`, and `docker rm` to manage them efficiently.

**Docker Run Commands and Features**

### Running Specific Versions of a Docker Image
- The `docker run redis` command runs a container with the latest version of Redis (e.g., 5.0.5 as of today).
- To run a specific version, specify the version tag:
  ```
  docker run redis:4.0
  ```
- If no tag is specified, Docker defaults to the `latest` tag.
- To check available versions, visit [Docker Hub](https://hub.docker.com/) and look up the image.

### Interactive Mode and Terminal Input
- By default, Docker containers do not listen to standard input and run in a non-interactive mode.
- To enable user input, use `-i` (interactive mode):
  ```
  docker run -i myapp
  ```
- However, this does not attach the terminal prompt.
- To also attach the terminal, use `-it` (interactive + pseudo-terminal):
  ```
  docker run -it myapp
  ```
  This enables both interaction and terminal prompts within the container.

### Port Mapping
- When running a web application inside a container, it typically listens on an internal port (e.g., 5000).
- To access it externally, map the container’s port to a port on the Docker host:
  ```
  docker run -p 80:5000 mywebapp
  ```
  This maps port 80 on the host to port 5000 inside the container.
- The container has an internal IP (e.g., 172.17.0.2), but it is only accessible from within the Docker host.
- Users outside the Docker host can access the application via the host’s IP (e.g., 192.168.1.5:80).
- Multiple applications can be mapped to different ports:
  ```
  docker run -p 3306:3306 mysql
  docker run -p 8306:3306 mysql
  ```
  However, the same port on the host cannot be mapped more than once.

### Persisting Data in Docker Containers
- Docker containers have isolated file systems. Any data created inside the container is lost when the container is deleted.
- Example, everytime u run jenkins, it will come from start tot setup, but if u map a volume to /var/jenkins_home then data is shared to all the conatiners
- To persist data, use volume mapping:
  ```
  docker run -v /opt/data_dir:/var/lib/mysql mysql
  docker run -p 8080:8080  -v /root/myjenkins:/var/jenkins_home -u root jenkins
  ```
- This mounts `/opt/data_dir` from the Docker host to `/var/lib/mysql` inside the container, ensuring data persists even if the container is deleted.

### Inspecting Containers
- To get detailed information about a container:
  ```
  docker inspect <container_id>
  ```
- Returns JSON-formatted data, including:
  - State
  - Mounts
  - Configuration
  - Network settings

### Viewing Container Logs
- If a container runs in detached mode (`-d`), logs are not immediately visible.
- To view logs:
  ```
  docker logs <container_id>
  ```
- Displays standard output from the container.



