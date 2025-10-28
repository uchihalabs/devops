
### **AIM:**

To install **Docker**, create a **Docker image** for an **Nginx web server**, serve static content from it, and **push the image to Docker Hub** for public or private access.

---

### **THEORY:**

**Docker** is a platform used for **containerization** — packaging an application and all its dependencies into a **lightweight, portable container** that can run anywhere (on any system with Docker installed).

- **Container**: A lightweight, isolated environment that runs your application.
    
- **Image**: A read-only template used to create containers.
    
- **Dockerfile**: A script containing instructions for building a Docker image.
    
- **Docker Hub**: A cloud-based repository for storing and sharing Docker images.
    

Using Docker ensures that an application runs the same way across all environments — development, testing, and production.

---

### **PROCEDURE:**

#### **1. Install Docker**

For **Ubuntu/Debian-based systems:**

**Step 1 – Update packages:**

```bash
sudo apt update
```

**Step 2 – Install required dependencies:**

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

**Step 3 – Add Docker’s official GPG key:**

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

**Step 4: Add Debian bookworm repo manually:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian bookworm stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

**Step 5: Update package index and install Docker:

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**Step 6 – Start and enable Docker:**

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

**Step 7 – Verify Docker installation:**

```bash
docker --version
sudo systemctl status docker
```

✅ If Docker is installed correctly, it will display something like:

```
Docker version 27.0.2, build e9f1c84
```

---

#### **2. Set Up a Docker Hub Account**

Go to [https://hub.docker.com](https://hub.docker.com), sign up (or log in), and note your **username** — it will be used later for tagging and pushing your image.

---

#### **3. Prepare Static Website Files**

Create a folder for the project:

```bash
mkdir my-nginx-app
cd my-nginx-app
```

Inside it, create a subfolder for your HTML files:

```bash
mkdir html
```

Create a sample webpage:

```bash
nano html/index.html
```

Paste this content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Welcome to Nginx</title>
</head>
<body>
  <h1>Hello, Nginx is running in Docker!</h1>
</body>
</html>
```

---

#### **4. Create a Dockerfile**

In the root project directory (`my-nginx-app/`), create a file named **Dockerfile**:

```bash
nano Dockerfile
```

Add the following instructions:

```dockerfile
FROM nginx:alpine
COPY ./html /usr/share/nginx/html
EXPOSE 80
```

**Explanation:**

- `FROM nginx:alpine` → Uses a lightweight version of Nginx as the base image.
    
- `COPY ./html /usr/share/nginx/html` → Copies your local `html` folder into Nginx’s web directory.
    
- `EXPOSE 80` → Exposes port 80 so the container can be accessed via HTTP.
    

---

#### **5. Build the Docker Image**

Build your Docker image using:

```bash
docker build -t <dockerhub-username>/nginx-website:latest .
```

Example:

```bash
docker build -t vishnu2004/nginx-website:latest .
```

**Explanation:**

- `-t` assigns a tag (name) to your image.
    
- `.` means build from the current directory (where Dockerfile is located).
    

---

#### **6. Test the Docker Image Locally**

Run your container:

```bash
sudo docker run -d -p 8080:80 <dockerhub-username>/nginx-website:latest
```

Then open a browser and visit:  
👉 **[http://localhost:8080](http://localhost:8080)**

You should see:

> “Hello, Nginx is running in Docker!”

**Explanation:**

- `-d` runs the container in the background (detached mode).
    
- `-p 8080:80` maps your local port 8080 to the container’s port 80.
    

---

#### **7. Login to Docker Hub**

Authenticate Docker CLI:

```bash
docker login
```

Enter your Docker Hub **username** and **password**.

---

#### **8. Push the Image to Docker Hub**

Push your image to Docker Hub:

```bash
docker push <dockerhub-username>/nginx-website:latest
```

Example:

```bash
docker push vishnu2004/nginx-website:latest
```

Once done, go to **[https://hub.docker.com/](https://hub.docker.com/)** → you’ll see your image listed under your account.

---

#### **9. Verify the Image from Anywhere**

You (or anyone else) can now pull and run the image using:

```bash
docker run -d -p 8080:80 <dockerhub-username>/nginx-website:latest
```

This confirms the image is successfully **containerized** and **publicly available** on Docker Hub.

---
### OUTPUT:

open a browser and visit:  
👉 **[http://localhost:8080](http://localhost:8080)**

You should see:

> “Hello, Nginx is running in Docker!”

### **RESULT:**

Docker was successfully installed, a custom **Nginx-based Docker image** was built to serve static web content, and the image was pushed to **Docker Hub** for sharing and deployment.

---

### **VIVA QUESTIONS:**

1. **What is Docker?**  
    Docker is a containerization platform that packages applications and their dependencies into isolated containers.
    
2. **What is a Docker container?**  
    A container is a lightweight, standalone runtime environment for running an application.
    
3. **What is a Docker image?**  
    A Docker image is a blueprint or template used to create containers.
    
4. **What is a Dockerfile?**  
    A text file that contains a series of instructions used to build a Docker image.
    
5. **What is Docker Hub?**  
    It’s a cloud-based registry service where you can store, share, and distribute Docker images.
    
6. **Why use `nginx:alpine` as a base image?**  
    Because it’s a lightweight version of Nginx that reduces image size and improves performance.
    
7. **What does `EXPOSE 80` mean?**  
    It declares that the container will listen on port 80 for incoming HTTP traffic.
    

---

