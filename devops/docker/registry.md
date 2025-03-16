## Docker Registry Overview 🚀

### What is a Docker Registry? 🏗️
A **Docker Registry** is the central repository where Docker images are stored. If containers were the rain ☔, they would rain from the Docker registry, which acts as the cloud ☁️.

### Running an Nginx Container 🖥️
To run an instance of the **Nginx** image, use the following command:
```sh
docker run nginx
```
This pulls the **nginx** image from the registry and runs a container.

### Understanding Image Naming 🏷️
Docker images follow a naming convention:
- The repository name: `nginx/nginx`
- The first part represents the **user or account name**
- If no account or repository name is provided, it defaults to the given name (e.g., `nginx`)

Your username is usually your **Docker Hub account name**, or if it's an organization, it will reflect the **organization's name**.

### Where Are Images Stored? 🗄️
If no specific location is mentioned, Docker assumes the image is stored in **Docker Hub** (default registry: `docker.io`). Other popular registries include:
- **Google Container Registry (GCR)**: `gcr.io`
- **Amazon Elastic Container Registry (ECR)**
- **Microsoft Azure Container Registry (ACR)**

### Pushing and Pulling Images 🔄
Images are stored in the registry and can be:
- **Pushed** when a new image is created or updated.
- **Pulled** when an application is deployed.

### Private vs Public Registries 🔐
Public registries store images accessible by everyone. However, for internal applications, organizations can use **private registries**:
- Cloud providers like **AWS, Azure, and GCP** offer **private registries**.
- Private registries require authentication for access.

To use a private registry:
1. **Login** to your private registry:
   ```sh
   docker login my-private-registry.com
   ```
2. **Run an image** from a private registry:
   ```sh
   docker run my-private-registry.com/my-image
   ```
If not logged in, an error stating "image not found" will appear.

### Hosting a Private Registry 🏠
If running applications on-premise, you may need to **host a private registry** internally.
- The **Docker Registry** itself is available as a **Docker image** (`registry`).
- It runs on **port 5000**.

#### Deploying a Private Registry 📦
1. **Run the Docker Registry**:
   ```sh
   docker run -d -p 5000:5000 --name registry registry
   ```
2. **Tag your image with the private registry URL**:
   ```sh
   docker tag my-image localhost:5000/my-image
   ```
3. **Push the image to the private registry**:
   ```sh
   docker push localhost:5000/my-image
   ```
4. **Pull the image from another machine in the network**:
   ```sh
   docker pull localhost:5000/my-image
   ```

### Deleting an Image from a Private Registry 🗑️
By default, Docker Registry does not support direct image deletion. However, you can:
1. Enable the deletion feature by adding `REGISTRY_STORAGE_DELETE_ENABLED=true`:
   ```sh
   docker run -d -p 5000:5000 --name registry -e REGISTRY_STORAGE_DELETE_ENABLED=true registry
   ```
2. Delete a specific image manifest:
   ```sh
   curl -X DELETE http://localhost:5000/v2/my-image/manifests/<digest>
   ```
   To find the `<digest>`, run:
   ```sh
   curl -s http://localhost:5000/v2/my-image/tags/list | jq
   ```
3. Garbage collect unused layers:
   ```sh
   docker exec -it registry bin/registry garbage-collect /etc/docker/registry/config.yml
   ```

### Conclusion 🎯
Now that you've learned about **Docker Registries**, proceed to the practice test to try working with **private registries**. Happy coding! 🚀

![image](https://github.com/user-attachments/assets/325b494a-e81a-472a-ad2a-85bddad87879)
![image](https://github.com/user-attachments/assets/c858b235-1174-4b51-98fd-27a1774ff37b)
