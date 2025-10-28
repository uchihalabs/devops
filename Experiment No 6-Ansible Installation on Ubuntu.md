### **AIM:**

To install and configure Ansible on Ubuntu by setting up a local control node and connecting it to a managed node within the same system or local network.

---

### **PROCEDURE:**

#### **Step 1:**

Create a new user for Ansible on your control node:

```bash
sudo adduser ansible
```

#### **Step 2:**

Give administrative privileges to the new user:

```bash
sudo usermod -aG sudo ansible
```

#### **Step 3:**

Switch to the new user:

```bash
sudo su ansible
```

#### **Step 4:**

Generate an SSH key pair:

```bash
ssh-keygen
```

#### **Step 5:**

Add your public key to the authorized keys file:

```bash
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
```

This lets Ansible connect to your own system via SSH.

for test:
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

```bash
ssh localhost
```

#### **Step 6:**

Install Ansible on the control node:

```bash
sudo apt update
sudo apt install ansible -y
```

#### **Step 7:**

Check Ansible version:

```bash
ansible --version
```

#### **Step 8:**

Set up your inventory file:

```bash
sudo mkdir -p /etc/ansible
sudo nano /etc/ansible/hosts
```

Add this content:

```
[local]
localhost ansible_connection=local
```

#### **Step 9:**

Check inventory and connectivity:

```bash
ansible-inventory --list -y
ansible all -m ping
```

### OUTPUT:

```
127.0.0.1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

### **RESULT:**

Ansible was successfully installed on the Ubuntu system.  
The control node established a connection with the localhost using SSH, and the ping module confirmed communication successfully.

---
