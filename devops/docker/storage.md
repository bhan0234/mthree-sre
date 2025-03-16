# 🚀 Advanced Docker Concepts: Storage Drivers & File Systems

## 📌 Introduction
Welcome to this lecture on **advanced Docker concepts**! Today, we will explore **Docker storage drivers** and **file systems**, understanding how Docker stores and manages data within containers. 🐳

---

## 📂 Where Does Docker Store Data?
When you install Docker, it creates a structured directory under:
```bash
/var/lib/docker
```
This directory contains multiple folders, such as:
- **`containers/`** ➝ Stores files related to running containers 🏗️
- **`image/`** ➝ Stores Docker image data 📦
- **`volumes/`** ➝ Stores persistent data created by containers 💾

---

## 🏗️ Docker's Layered Architecture
Docker images are **built in layers**, meaning each instruction in a `Dockerfile` creates a new layer:
1️⃣ **Base Layer** (e.g., Ubuntu OS 🖥️)
2️⃣ **Install Packages** (e.g., APT packages 📜)
3️⃣ **Dependencies** (e.g., Python & Flask 🐍)
4️⃣ **Application Source Code** (📝)
5️⃣ **Entry Point** (🚀)

### 🎯 Benefits of Layered Architecture
- **Efficient Image Building** ➝ Reuses unchanged layers 🔁
- **Faster Builds** ➝ Uses cached layers 🏎️
- **Saves Disk Space** ➝ Reduces duplication 📉

For example, if you build two applications that share the same base image and dependencies, Docker will **reuse** the existing layers, saving time and storage.

---

## 📜 Read-Only & Writable Layers
- **Image Layers** are **read-only** 🛑
- **Container Layer** is **writable** ✏️

When a container is created, a **new writable layer** is added **on top** of the image layers.
- Any **file modifications** are copied to the writable layer.
- This is called the **Copy-on-Write (COW) mechanism** 📑
- When the container is **deleted**, this writable layer **disappears** 🗑️

---

## 📌 Data Persistence in Docker
When containers are removed, any data in the writable layer is lost! To persist data, we use **Docker Volumes**:

### 📂 **Volume Mounting**
1️⃣ Create a volume:
```bash
docker volume create data_volume
```
2️⃣ Run a container with the volume:
```bash
docker run -v data_volume:/var/lib/mysql mysql
```

Now, the database data is stored **outside** the container in `/var/lib/docker/volumes/` and persists even if the container is deleted. ✅

### 📂 **Bind Mounting**
If you already have a directory with data on your host system, you can **bind mount** it:
```bash
docker run -v /data/mysql:/var/lib/mysql mysql
```
This mounts **existing host data** into the container.

---

## 📌 Newer Syntax for Mounting
Instead of `-v`, Docker recommends using `--mount`:
```bash
docker run /
 --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql
```
- `type=bind` ➝ Indicates a bind mount
- `source=/data/mysql` ➝ The host path
- `target=/var/lib/mysql` ➝ The container path

---

## ⚙️ Docker Storage Drivers
Docker uses **storage drivers** to manage the layered file system.

### 🔹 Common Storage Drivers:
- **AUFS** ➝ Default for Ubuntu 🐧
- **Overlay & Overlay2** ➝ Recommended for modern Linux versions
- **Device Mapper** ➝ Used on RHEL, CentOS
- **Btrfs & ZFS** ➝ Advanced file systems

### 🔹 How Does Docker Choose a Storage Driver?
- Docker **automatically selects** the best available storage driver based on your **OS** and **file system**.
- You can **manually configure** the storage driver if required for performance optimization. ⚙️

---

## 🎯 Conclusion
✅ Docker follows a **layered architecture** for efficiency and storage optimization.
✅ Data in the container's writable layer is **temporary**; to persist, use **volumes or bind mounts**.
✅ **Storage drivers** enable the layered architecture and differ based on OS & performance needs.

![Docker Logo](https://github.com/user-attachments/assets/3cf26ce1-cbf6-4002-b9d8-4ede9d6fe6a4)
![Docker Logo](https://github.com/user-attachments/assets/11007a51-5b8b-4928-9549-079539944762)
![Docker Logo](https://github.com/user-attachments/assets/dc68bd6d-c2d5-4ef1-86ac-64cd20ff21dc)

