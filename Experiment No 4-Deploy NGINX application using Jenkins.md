
### **AIM**

To automate the deployment of an Nginx application using Jenkins, ensuring **Continuous Integration and Continuous Deployment (CI/CD)** by leveraging Jenkins pipelines to build and deploy Dockerized applications automatically.

---

### ⚙️ **TOOLS / SOFTWARE REQUIRED**

- **Operating System:** Ubuntu / Kali / Debian-based Linux
    
- **Software:**
    
    - Docker
        
    - Jenkins
        
    - Git
        
    - Web browser (for Jenkins dashboard access)
        

---

### 💡 **CONCEPT**

Jenkins is an **automation server** that helps in automating software build, testing, and deployment processes.

By integrating **Docker with Jenkins**, you can:

- Automatically build Docker images from source code.
    
- Run containers as part of deployment.
    
- Maintain a consistent environment for development, testing, and production.
    

In this experiment:

- Jenkins pulls code from GitHub.
    
- Builds a Docker image for an Nginx-based web app.
    
- Deploys it automatically inside a container.
    

---

### 🧰 **PROCEDURE**

#### **1. Install Docker**

```bash
sudo apt update
sudo apt install docker -y
sudo systemctl start docker
sudo systemctl enable docker
```

✅ Verify installation:

```bash
docker --version
```

---

#### **2. Install Jenkins**

Add Jenkins repository and GPG key:

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" \
| sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Install Jenkins and Java:

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre -y
sudo apt install jenkins -y
```

Start Jenkins:

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

---

#### **3. Access Jenkins**

Open browser:

```
http://<your-machine-ip>:8080
```

To get the admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Login using this password.

---

#### **4. Jenkins Initial Setup**

- Choose **“Install suggested plugins”** → Jenkins will install essential plugins.
    
- Create an admin user.
    
- Once setup completes, Jenkins dashboard will open.
    

---

#### **5. Create a New Project**

- Click on **“New Item”**
    
- Enter project name (e.g., `Nginx-Deploy`)
    
- Choose **“Freestyle project”** → Click **OK**
    

---

#### **6. Configure Source Code Management**

- Choose **Git**
    
- Enter your repository URL, e.g.:
    
    ```
    https://github.com/your-git-username/repo-name.git
    ```
    
- Set **Branch Specifier** to:
    
    ```
    */main
    ```
    

---

#### **7. Add Build Step**

- In **Build → Add build step → Execute shell**
    
- Add the following commands:
    

```bash
docker build -t nginx-jenkins-demo .
docker stop nginx-demo || true
docker rm nginx-demo || true
docker run -d --name nginx-demo -p 8081:80 nginx-jenkins-demo
```

---

#### **8. Save and Build**

- Click **Save**
    
- Then click **Build Now**
    
- Jenkins will:
    
    - Clone your GitHub repo
        
    - Build the Docker image
        
    - Deploy it as a running container
        

---

#### **9. Verify Deployment**

Open browser and visit:

```
http://localhost:8081
```

✅ You should see the Nginx web page served by your containerized application.

---

### 📊 **WORKING PRINCIPLE**

1. Jenkins pulls the source code from the GitHub repository.
    
2. Docker builds an image from the Dockerfile.
    
3. Jenkins automatically deploys a new container using that image.
    
4. The web application becomes available at the exposed port (8081).
    

This creates a **continuous deployment pipeline**—every time code is updated in GitHub, Jenkins can automatically rebuild and redeploy the app.

---

### OUTPUT:

Open browser and visit:

```
http://localhost:8081
```

You should see the Nginx web page served by your containerized application.

### 🧾 **RESULT**

The Nginx web application was successfully **deployed automatically using Jenkins**, demonstrating **CI/CD integration** where the entire build and deployment process is automated via Jenkins and Docker.

---

### If You have an Error while run the build follow the bellow steps:



Now the only problem is at this part:

```
ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock
```

---

## ⚠️ **What This Means**

Jenkins is trying to run Docker commands:

```bash
docker build -t custom-nginx .
```

…but the **Jenkins user doesn’t have permission** to access the Docker daemon socket (`/var/run/docker.sock`).

By default:

- Docker runs as **root**.
    
- Jenkins runs as a separate **system user (jenkins)**.  
    So when Jenkins tries to use Docker, it gets **permission denied**.
    

---

## ✅ **Fix – Give Jenkins Access to Docker**

### 🔹 Step 1: Check Docker group

Run this command:

```bash
grep docker /etc/group
```

If it exists, you’ll see something like:

```
docker:x:998:
```

If not, create it:

```bash
sudo groupadd docker
```

---

### 🔹 Step 2: Add Jenkins to the Docker group

```bash
sudo usermod -aG docker jenkins
```

This command adds the `jenkins` user to the `docker` group so it can access `/var/run/docker.sock`.

---

### 🔹 Step 3: Restart services

You must restart both **Docker** and **Jenkins** for the permission change to take effect:

```bash
sudo systemctl restart docker
sudo systemctl restart jenkins
```

---

### 🔹 Step 4: Verify permission

Switch to the Jenkins user and try Docker:

```bash
sudo su - jenkins
docker ps
```

If you see a list of containers (or an empty list without an error), ✅ it’s fixed.

Exit Jenkins shell:

```bash
exit
```

---

### 🔹 Step 5: Re-run Jenkins job

Go back to your Jenkins web UI →  
Click **“Build Now”** again.

✅ It should now build and run your Docker image successfully.

You’ll see output like:

```
Successfully built <image_id>
Successfully tagged custom-nginx:latest
d1f0c7a6f2f3dcb1...
Finished: SUCCESS
```

---

### 🧠 **Alternative Fix (Not Recommended for Production)**

If you want a quick temporary fix (for testing only):

```bash
sudo chmod 666 /var/run/docker.sock
```

That makes the Docker socket world-accessible (⚠️ insecure).  
Prefer the **group permission method** above for real setups.

---

### ✅ **Summary**

|Problem|Cause|Fix|
|---|---|---|
|Jenkins can’t access Docker|Jenkins user lacks permission|`sudo usermod -aG docker jenkins` + restart services|

---

Would you like me to show you how to automatically **deploy the container after every successful Jenkins build** (a mini CI/CD pipeline)?