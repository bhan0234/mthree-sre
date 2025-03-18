**Docker Engine Notes**

---

## **Introduction to Docker Engine** 🚀🐳🔧

Docker Engine is the core component of Docker that allows applications to run in isolated containers. It consists of three key components:

1. **Docker Daemon**: The Docker Daemon is a background process responsible for managing Docker objects such as containers, images, volumes, and networks. It listens for API requests and handles container lifecycle management, including starting, stopping, and monitoring containers.

**Example:**

- When you run `docker run nginx`, the Docker CLI sends this request to the daemon, which then pulls the Nginx image (if not already available) and starts a new container based on that image.
- The daemon ensures that the container is correctly isolated, assigns it a unique identifier, and manages resource allocation for its execution.

2. **Docker REST API Server**: The Docker REST API Server provides an API interface for interacting with the Docker daemon programmatically. It allows external applications, scripts, and services to communicate with Docker without using the command-line interface (CLI). This is useful for automation, remote management, and integration with other systems.

**Example:**

- A DevOps tool like Jenkins can use the Docker REST API to create and manage containers dynamically as part of a CI/CD pipeline.
- Running the following cURL command can retrieve a list of running containers using the API:

```sh
$ curl --unix-socket /var/run/docker.sock http://localhost/containers/json
```

This command queries the Docker daemon to fetch details about currently running containers.

3. **Docker CLI**: Command-line interface for executing Docker commands.

The Docker CLI does not need to be on the same system as the Docker Engine. You can use it remotely by specifying the `-H` option with the Docker host address.

**Example of Running a Container on a Remote Docker Host:**

```sh
$ docker -H=10.1.23.2:2375 run nginx
```

---

## **Docker Host** 🏠💻🌍

A **Docker Host** is any system where Docker Engine is installed and running. It could be:

- A **local machine** (e.g., your laptop or desktop)
- A **remote server** (e.g., an AWS EC2 instance, a dedicated Linux server)
- A **cloud-based container service** (e.g., AWS ECS, Google Cloud Run)

Since you are running Docker on an **EC2 Linux instance**, your **Docker Host is the EC2 instance** itself.

---

## **What Are Namespaces?** 🏗️🔒🔄

Namespaces are a Linux feature that Docker uses to provide isolation between containers. They ensure that each container operates in its own environment, separate from the host and other containers. Different types of namespaces include:

- **PID Namespace**: Gives each container its own process tree, making it appear as a separate system.
- **Network Namespace**: Assigns each container its own network stack.
- **Mount Namespace**: Isolates file system views for each container.
- **IPC Namespace**: Provides isolated interprocess communication.
- **UTS Namespace**: Allows a container to have its own hostname.

### **Process ID (PID) Namespace** 🆔📦🔍

Whenever a Linux system starts, it begins with a single process (PID 1), which is the root process that starts all other processes. If a container were truly independent, it would also need to have a PID 1 process as its root.

However, since containers share the same underlying host, their processes are actually running on the host itself. This means that **two processes cannot have the same process ID at the host level**. To solve this, Docker uses PID namespaces, allowing each container to think it has its own independent process tree.

Now, if we were to create a container, which is essentially like a child system within the current system, the container must believe that it is an independent system. It should have its own set of processes, originating from a root process with a process ID of 1.

But we know that there is no hard isolation between containers and the underlying host. The processes running inside the container are, in fact, running on the host itself. This means two processes **cannot have the same PID 1** at the host level.

**Example:**

- A container starts an Nginx server. Inside the container, Nginx runs as **PID 1**.
- On the host, the same process may have a different PID, such as **1234**.
- This isolation allows each container to function as if it were a separate system.

This can be observed by listing processes inside and outside the container:

```sh
$ docker run -d nginx
$ docker exec -it <container_id> ps aux
```

Inside the container, the Nginx service appears as PID 1. However, on the host system, it will have a different PID, reflecting that it is actually part of the overall Linux process tree.

---

## **How Docker Manages Resources Using Control Groups (cgroups)** ⚙️📊🚦

While namespaces isolate containers, **control groups (cgroups)** help manage how system resources (CPU, memory) are shared among them.

By default, there are no restrictions on how much CPU or memory a container can use, which means a single container could consume all system resources. To prevent this, Docker allows resource limits to be set using cgroups.

### **Limiting CPU and Memory Usage**

To restrict CPU and memory usage for a container:

```sh
$ docker run --cpus=0.5 --memory=100m nginx
```

- `--cpus=0.5` → Limits CPU usage to 50%
- `--memory=100m` → Restricts memory usage to 100MB

This ensures that containers do not interfere with the performance of the host system.

---


