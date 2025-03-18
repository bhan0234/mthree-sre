**Docker Commands, Arguments, and Entry Points**

### **Understanding Commands in Docker**
When you run a Docker container from an Ubuntu image using:
```sh
$ docker run ubuntu
```
- The container starts and exits immediately.
- If you check running containers, you won’t see it because it has already exited.
- Checking all containers (including stopped ones) shows that the new container is in an **exited state**.

### **Why Do Containers Exit?**
- Unlike virtual machines, containers **are not meant to host a full operating system**.
- Containers are designed to run **a specific process or task**.
- Examples:
  - Hosting a web server
  - Running a database
  - Performing computations
- A container only runs as long as its main process is alive. Once the process stops, the container **exits**.

### **What Defines the Process Inside a Container?**
- The **CMD** (Command) instruction in a Dockerfile specifies the default process.
- Example: In an **NGINX** image, the default command is:
  ```sh
  CMD ["nginx", "-g", "daemon off;"]
  ```
- For a **MySQL** image:
  ```sh
  CMD ["mysqld"]
  ```

### **Understanding Ubuntu Container Behavior**
- The Ubuntu image **defaults to running `bash`**.
- Bash is an **interactive shell**, not a long-running process.
- Since Docker **does not attach a terminal by default**, `bash` exits immediately.

### **Overriding the Default Command**
- You can specify a different command while running a container:
  ```sh
  docker run ubuntu sleep 5
  ```
  - This runs the `sleep` command for 5 seconds and then exits.
  - It **overrides** the default `CMD` in the image.

### **Making Changes Permanent**
- Instead of specifying a command manually, you can define it in a **custom Dockerfile**:
  ```dockerfile
  FROM ubuntu
  CMD sleep 5  (or)
  CMD ["sleep", "5"]
  ```
- Build the image:
  ```sh
  docker build -t ubuntu-sleeper .
  ```
- Now running the container will **always sleep for 5 seconds**:
  ```sh
  docker run ubuntu-sleeper
  ```

### **CMD vs. ENTRYPOINT**
#### **CMD Instruction**
- Specifies a **default command** for the container.
- Can be **overridden** by passing arguments in `docker run`.
- Example:
  ```dockerfile
  FROM ubuntu
  CMD ["sleep", "5"]
  ```
  - Running `docker run ubuntu-sleeper 10` **replaces** `sleep 5` with `sleep 10`.

#### **ENTRYPOINT Instruction**
- Defines the **fixed** command to be executed.
- Arguments passed during `docker run` **get appended** to ENTRYPOINT.
- Example:
  ```dockerfile
  FROM ubuntu
  ENTRYPOINT ["sleep"]
  CMD ["5"]
  ```
  - Running `docker run ubuntu-sleeper 10` executes `sleep 10`.
  - Running `docker run ubuntu-sleeper` executes `sleep 5` (default value).

### **Using Both ENTRYPOINT and CMD**
- CMD provides **default arguments** if none are supplied.
- ENTRYPOINT ensures the **core command remains unchanged**.
- Always specify them in **JSON format**:
  ```dockerfile
  FROM ubuntu
  ENTRYPOINT ["sleep"]
  CMD ["5"]
  ```
  - `docker run ubuntu-sleeper` → `sleep 5`
  - `docker run ubuntu-sleeper 10` → `sleep 10`

### **Overriding ENTRYPOINT at Runtime**
- If you want to completely replace ENTRYPOINT, use:
  ```sh
  docker run --entrypoint sleep2.0 ubuntu-sleeper 10
  ```
  - This runs `sleep2.0 10` instead of `sleep 10`.

### **Conclusion**
- **CMD**: Provides default command **but can be overridden**.
- **ENTRYPOINT**: Defines the main process **and appends arguments**.
- **Using both** allows flexibility, ensuring a default value while allowing overrides.

This knowledge is crucial for structuring **Dockerfiles** efficiently!

