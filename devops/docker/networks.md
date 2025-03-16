# 🚀 Networking in Docker

## 📌 Introduction

When you install Docker, it automatically creates three networks:

- **Bridge**
- **Null**
- **Host**

By default, a container gets attached to the **Bridge** network. If you need to associate a container with another network, you can specify the network information using the network command-line parameter.

---

## 🌉 Bridge Network

The **Bridge Network** is a private internal network created by Docker on the host. All containers attached to this network receive an internal IP address, usually in the `172.17.x.x` range.

To run a container using the bridge network explicitly, use:

```sh
docker run ubuntu
```

🔹 Containers can communicate with each other using their internal IP addresses.
🔹 To access containers from outside, map the ports of the containers to the host machine.

---

## 🌍 Host Network

If you associate a container with the **Host Network**, it removes any network isolation between the Docker host and the container.

🔹 Example: Running a container with the host network:

```sh
docker run ubuntu --network=host
```

🔹 Example: Running a web server on port `5000` in a web container makes it directly accessible externally without port mapping.
🔹 Limitation: You cannot run multiple containers on the same port when using the host network.

---

## 🚫 None Network

Containers in the **None Network** are completely isolated:

```sh
docker run ubuntu --network=none
```

🔹 They are **not attached** to any network.
🔹 They **cannot** communicate with other containers or external networks.

---

## 🏗️ Creating Custom Networks

By default, Docker creates only one internal **Bridge Network** (`172.17.x.x`). However, you can create custom networks to isolate containers.

### 🔹 Creating a Custom Bridge Network

Use the following command:

```sh
docker network create \
--driver bridge 
--subnet 192.168.1.0/24 my_custom_network
```

To list all networks:

```sh
docker network ls
```

---

## 🔍 Viewing Network Settings

To inspect the network settings and assigned IP of a container, use:

```sh
docker inspect <container_id>
```

This displays:

- Network type
- Internal IP address
- MAC address
- Other settings

---

## 🛠️ Container Communication via Name

Containers can reach each other **using their names** instead of IP addresses.

🔹 Example: A **web server** accessing a **MySQL database container**

Instead of using the IP (`172.17.0.3`), use the container name:

```sh
mysql.connect(mysql)
```

Docker has a built-in **DNS server** (`127.0.0.11`) that helps resolve container names.

---

## 🏗️ How Docker Implements Networking

Docker uses **network namespaces** to isolate containers within the host. It connects containers using **virtual ethernet pairs (veth)**.

---

## 🎯 Summary

- **Bridge Network** → Default, internal communication, requires port mapping for external access.
- **Host Network** → No isolation, containers share the host's network.
- **None Network** → Fully isolated, no external or container-to-container communication.
- **Custom Networks** → Created using `docker network create`.
- **Container Communication** → Use container names instead of IPs for stability.

✅ Now, head over to the **practice test** and try out networking in Docker! 🎯

See you in the next lecture! 🚀

![Docker Logo](https://github.com/user-attachments/assets/3dd0a300-3f1e-41e3-a28a-e94f95e8fea9)
![Docker Logo](https://github.com/user-attachments/assets/b6c42f96-c9ff-481b-9f2e-27362e0c15cb)
![Docker Logo](https://github.com/user-attachments/assets/e1fa100c-a3df-4c33-9daf-df83ef9f79f6)

