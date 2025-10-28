### **AIM**

To install Grafana on Ubuntu is to set up a robust data visualization and monitoring tool.

---

### **REQUIREMENTS**

- Ubuntu system with internet access
    
- Sudo privileges
    
- Browser (e.g., Firefox or Chrome)
    

---

### **PROCEDURE**

#### **Step 1: Add Grafana APT Repository**

Run the following commands to add the official Grafana repository:

```bash
sudo apt install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```

#### **Step 2: Install Grafana**

Update the package list and install Grafana:

```bash
sudo apt update
sudo apt install grafana -y
```

#### **Step 3: Start and Enable Grafana Service**

Start Grafana and enable it to start automatically at boot:

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

#### **Step 4: Access Grafana Web Interface**

1. Open a web browser.
    
2. Navigate to the URL:
    
    ```
    http://localhost:3000
    ```
    
3. Log in with the default credentials:
    
    - **Username:** admin
        
    - **Password:** admin
        
4. Change the password when prompted and explore the Grafana dashboard.
    

---

### OUTPUT:

Open browser and visit:

```
http://localhost:3000
```

 You should see the Grafana Login Page
### **RESULT**

Grafana was successfully installed and configured on Ubuntu. The Grafana service started successfully

