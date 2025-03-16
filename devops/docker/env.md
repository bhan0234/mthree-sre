**Understanding Environment Variables in Docker**

### **What are Environment Variables?**
Environment variables are key-value pairs used to configure applications without modifying the code. These variables can store **configuration settings**, **API keys**, **database credentials**, and other values that might change depending on the environment.

### **Types of Environment Variables**
1. **Secret Variables**: Used to store sensitive information like API keys and passwords.
2. **Configuration Variables**: Used to define settings like app themes, background colors, or log levels.
3. **System Variables**: Predefined variables that provide system-related information (e.g., `PATH`, `HOME`).

---

### **Using Environment Variables in a Docker Container**

#### **Example: A Simple Flask Web Application**
Consider a Python-based web application that displays a web page with a background color.

```python
from flask import Flask
import os

app = Flask(__name__)

@app.route('/')
def home():
    app_color = os.getenv('APP_COLOR', 'red')  # Default color is red
    return f'<body style="background-color: {app_color};">Hello, World!</body>'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**Problem:**
The background color is hardcoded to **red**. If we want to change it, we must edit the code.

**Solution:**
Instead of modifying the application code, we use an **environment variable**.

---

### **Defining Environment Variables in Docker**

#### **1️⃣ Setting Environment Variables in a Dockerfile**
A `Dockerfile` can include environment variables using the `ENV` instruction:

```dockerfile
FROM python:3.9

WORKDIR /app

COPY . .

RUN pip install flask

ENV APP_COLOR=red  # Setting an environment variable

CMD ["python", "app.py"]
```

After building the Docker image:
```sh
docker build -t my-app .
docker run -p 5000:5000 my-app  # Runs the app with a red background
```

#### **2️⃣ Overriding Environment Variables During `docker run`**
We can override `APP_COLOR` using the `-e` option in `docker run`:

```sh
docker run -p 5000:5000 -e APP_COLOR=blue my-app
```
Now the application runs with a **blue** background.

#### **3️⃣ Using a `.env` File to Store Environment Variables**
Instead of specifying environment variables in commands, use a `.env` file:

**`.env` file:**
```
APP_COLOR=green
```

Then, run the container using:
```sh
docker run --env-file .env -p 5000:5000 my-app
```
Now, the application runs with a **green** background.

---

### **Inspecting Environment Variables in a Running Container**
To check environment variables set inside a container, use:
```sh
docker inspect <container_id>
```
Find them under the **Config** section.

---

### **Key Takeaways**
✔️ Environment variables help separate **configuration** from **application code**.
✔️ They can be set in a **Dockerfile**, overridden at runtime, or stored in a `.env` file.
✔️ To deploy multiple containers with different configurations, use different environment variables.
✔️ Use `docker inspect` to view environment variables inside a running container.

By using environment variables effectively, we can make our applications more **flexible, secure, and easier to maintain**.

