### **AIM**

To install and configure **Prometheus** on Ubuntu for system monitoring and metrics collection.

---

### **REQUIREMENTS**

- Ubuntu system with internet access
    
- Sudo privileges
    
- Web browser (to access Prometheus dashboard)
    

### **PROCEDURE**

#### **Step 1: Update System Packages**

Before installation, update all existing packages:

```bash
sudo apt update
sudo apt upgrade -y
```

---

#### **Step 2: Create a Prometheus System User**

To improve security, create a dedicated Prometheus user and directories:

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

---

#### **Step 3: Download Prometheus**

Visit the [official Prometheus download page](https://prometheus.io/download/) or run:

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.51.0/prometheus-2.51.0.linux-amd64.tar.gz
```

Extract the downloaded file:

```bash
tar xvf prometheus-2.51.0.linux-amd64.tar.gz
cd prometheus-2.51.0.linux-amd64
```

---

#### **Step 4: Move Prometheus Files**

Move the binary files to system directories:

```bash
sudo mv prometheus /usr/local/bin/
sudo mv promtool /usr/local/bin/
```

Copy the configuration files:

```bash
sudo mv consoles /etc/prometheus
sudo mv console_libraries /etc/prometheus
sudo mv prometheus.yml /etc/prometheus
```

---

#### **Step 5: Set Permissions**

```bash
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
```

---

#### **Step 6: Create a Systemd Service File**

Create a new service file for Prometheus:

```bash
sudo nano /etc/systemd/system/prometheus.service
```

Paste the following content:

```ini
[Unit]
Description=Prometheus Monitoring
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/ \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

Save and exit.

---

#### **Step 7: Start and Enable Prometheus**

```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```

---

#### **Step 8: Verify the Installation**

Check the service status:

```bash
sudo systemctl status prometheus
```

If it is **active (running)**, Prometheus is successfully installed.

---

### OUTPUT:

Open a web browser and visit:

```
http://localhost:9090
```

You should see the Prometheus dashboard interface.

---

### **RESULT**

Prometheus was successfully installed and configured on Ubuntu. The service started correctly, and the dashboard was accessible via **[http://localhost:9090](http://localhost:9090)**.
