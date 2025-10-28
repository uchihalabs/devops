## **Ex.No: 12 – CREATE CI PIPELINE USING JENKINS**

### **AIM**

To create and configure a Continuous Integration (CI) pipeline using Jenkins, enabling automatic build, test, and integration processes whenever code changes are pushed to Git.

---

### **THEORY (short and non-boring)**

CI means every time you push code to your repo, Jenkins automatically:

1. Fetches your latest code.
    
2. Builds it.
    
3. Runs tests (and yells if something breaks).
    
4. Optionally deploys the working build.
    

Basically, it’s your safety net for coding chaos.

---

### **PROCEDURE**

#### **1. Prerequisites**

- Jenkins must already be installed and running.
    
- Your Git repo (e.g. your NGINX repo) must exist and be working.
    
- You should already have Git and Docker installed (you do).
    
- Install these Jenkins plugins if not already present:
    
    - **Git Plugin**
        
    - **Pipeline Plugin**
        
    - (Optional) **Blue Ocean** for a fancy UI
        

---

#### **2. Create a new Jenkins pipeline**

1. Open Jenkins dashboard → **New Item**
    
2. Name it something like `ci-pipeline-demo`
    
3. Select **Pipeline**
    
4. Click **OK**
    

---

#### **3. Configure source and pipeline**

In the config page:

- Scroll to **Pipeline → Definition**
    
- Choose **Pipeline script from SCM**
    
- SCM: **Git**
    
- Paste your repo URL:
    
    ```
    https://github.com/your-github-username/your_repo_name.git
    ```
    
- Select credentials (GitHub PAT)
    
- Branch: `*/main`
    
- Jenkins will now look for a file named `Jenkinsfile` inside your repo.
    

---

#### **4. Create the Jenkinsfile**

In your **local repo folder** (the same one you pushed earlier), create a file called:

```
Jenkinsfile
```

Add this:

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/KowshikT/Jen.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t ci-demo-nginx .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm ci-demo-nginx nginx -t'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                docker stop ci-demo-container || true
                docker rm ci-demo-container || true
                docker run -d --name ci-demo-container -p 8082:80 ci-demo-nginx
                '''
            }
        }
    }
}
```

This pipeline:

- Pulls your repo
    
- Builds your Docker image
    
- Runs a simple NGINX config test
    
- Deploys the container on port `8082`
    

---

#### **5. Push the Jenkinsfile to GitHub**

From your local repo:

```bash
git add Jenkinsfile
git commit -m "Added Jenkinsfile for CI pipeline"
git push
```

---

#### **6. Run the pipeline**

Go back to Jenkins → open your pipeline → click **Build Now**.  
Watch the stages execute: Checkout → Build → Test → Deploy.  
If it goes green, you’ve nailed it.

Then visit:

```
http://<your-vm-ip>:8082
```

and you’ll see your web page served by your CI-built container.

---

### **OUTPUT**


- Jenkins pipeline stages (Checkout, Build, Test, Deploy)
    
- The site running on port 8082 in the browser.
    

---

### **RESULT**

Thus, a Continuous Integration (CI) pipeline was successfully created and executed using Jenkins. The build and deployment were automatically handled upon code changes.

---

