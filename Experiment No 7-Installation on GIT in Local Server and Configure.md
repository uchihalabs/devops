### **AIM:**

To install Git on a local server and configure it with GitHub for version control.  
Additionally, to install Jenkins on a cloud instance and integrate it for Continuous Deployment (CD).

---

## ⚙️ **PROCEDURE**

### **PART A — Install and Configure Git**

#### **Step 1: Install Git**

Install Git on your local system using the following command:

```bash
sudo apt install git -y
```

Git is now installed and ready to be configured.

---

#### **Step 2: Configure Git with GitHub Account**

Set up your global Git configuration so that your commits are associated with your GitHub account:

```bash
git config --global user.name "your-username"
git config --global user.email "youremail@gmail.com"
```

You can verify the configuration using:

```bash
git config --list
```

---

#### **Step 3: Create a New Repository on GitHub**

- Log in to your [GitHub account](https://github.com).
    
- Click on **New Repository**.
    
- Give a repository name (e.g., `my-first-repo`) and click **Create Repository**.
    

---

#### **Step 4: Upload Code to GitHub**

Initialize Git in your project directory and push your files:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/your-username/my-first-repo.git
git push -u origin main
```

---

#### **Step 5: Verify Upload**

Check your GitHub repository — your project files should now appear in the repository.

---

## ☁️ **PART B — Install Jenkins in Cloud Instance**


---

#### **1. Install Jenkins**

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

#### **2. Access Jenkins**

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

#### **3. Jenkins Initial Setup**

- Choose **“Install suggested plugins”** → Jenkins will install essential plugins.
    
- Create an admin user.
    
- Once setup completes, Jenkins dashboard will open.
    

---

#### **4. Create a New Project**

- Click on **“New Item”**
    
- Enter project name (e.g., `Nginx-Deploy`)
    
- Choose **“Freestyle project”** → Click **OK**
    

---

#### **5. Configure Source Code Management**

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

#### **6. Add Build Step**

- In **Build → Add build step → Execute shell**
    
- Add the following commands:
    

```bash
docker build -t nginx-jenkins-demo .
docker stop nginx-demo || true
docker rm nginx-demo || true
docker run -d --name nginx-demo -p 8081:80 nginx-jenkins-demo
```

---

#### **7. Save and Build**

- Click **Save**
    
- Then click **Build Now**
    
- Jenkins will:
    
    - Clone your GitHub repo
        
    - Build the Docker image
        
    - Deploy it as a running container
        

---

### OUTPUT:

Open browser and visit:

```
http://localhost:8081
```

 You should see the Nginx web page served by your containerized application.

---

## **RESULT:**

Thus, **Git was successfully installed and configured with GitHub** for version control.  
Jenkins was installed on a **cloud instance**, integrated with GitHub, and a **Continuous Deployment (CD) pipeline** was created and tested successfully.
