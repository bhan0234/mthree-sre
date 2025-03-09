# 📌 Important Docker Commands and Concepts 🐳

## 🚀 Docker Run Command
- `docker run <image>`: Runs a container from an image.
- `docker run -it ubuntu bash` : allowing you to access the container’s terminal 
- -i (interactive mode) 🖥️: Keeps STDIN open so you can provide input to the container.
- -t (pseudo-TTY) 📟: Allocates a terminal session, making it more user-friendly.
- --name webapp 
- If the image is not present, Docker pulls it from **Docker Hub**.
- Subsequent executions reuse the same image.

## 📜 Listing Containers
- `docker ps`: Lists all **running** containers with details like ID, image, status, and name.
- `docker ps -a`: Lists **all containers**, including stopped ones.

## ⏹️ Stopping and Removing Containers
- `docker stop <container_id/name>`: Stops a running container. prints the container after succesful.
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

🛠️ Next, let's practice these commands in the **Docker CLI lab environment**! 🚀

