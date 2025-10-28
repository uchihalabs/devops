### **AIM:**

To create an Ansible role and playbook to automate the installation and configuration of **NGINX** for web server deployment.

---

### **PROCEDURE:**

**Step 1:** Create a directory for your Ansible project

```bash
mkdir ansible_project
cd ansible_project
```

---

**Step 2:** Create a new Ansible role

```bash
ansible-galaxy init nginx_role
```

This command generates a directory structure with predefined folders for **tasks**, **handlers**, **variables**, **files**, and **templates**.

---

**Step 3:** Define default variables  
Edit the file:

```bash
cd nginx_role/defaults/
nano main.yml
```

**Example content:**

```yaml
---
nginx_port: 80
```

---

**Step 4:** Create a static HTML file  
Go to the files directory and create an index page:

```bash
cd ../files
nano index.html
```

**Example content:**

```html
<html>
  <head><title>Welcome to Ansible NGINX</title></head>
  <body><h1>NGINX deployed using Ansible!</h1></body>
</html>
```

---

**Step 5:** Add service handlers  
Edit the file:

```bash
cd ../handlers
nano main.yml
```

**Example content:**

```yaml
---
- name: Restart NGINX
  service:
    name: nginx
    state: restarted
```

---

**Step 6:** Define role metadata  
Edit the file:

```bash
cd ../meta
nano main.yml
```

**Example content:**

```yaml
---
galaxy_info:
  author: your_name
  description: Install and configure NGINX
  license: MIT
  min_ansible_version: 2.9
```

---

**Step 7:** Define main tasks for installation and configuration  
Edit the file:

```bash
cd ../tasks
nano main.yml
```

**Example content:**

```yaml
---
- name: Install NGINX
  apt:
    name: nginx
    state: present
    update_cache: yes

- name: Copy index.html to NGINX root
  copy:
    src: index.html
    dest: /var/www/html/index.html

- name: Start and enable NGINX service
  service:
    name: nginx
    state: started
    enabled: yes
```

---

**Step 8:** Create a playbook to call the role  
In the project root folder:

```bash
cd ../../
nano playbook.yml
```

**Example content:**

```yaml
---
- hosts: webservers
  become: yes
  roles:
    - nginx_role
```

---

**Step 9:** Create an inventory file

```bash
nano inventory
```

**Example content:**

```
[webservers]
localhost ansible_connection=local
```

---

**Step 10:** Run the playbook

```bash
ansible-playbook -i inventory playbook.yml --ask-become-pass
```

---

### **OUTPUT:**

After execution, Ansible will display the task summary as:

```
PLAY [webservers] *********************************************************
TASK [Gathering Facts] ****************************************************
ok: [localhost]
TASK [nginx_role : Install NGINX] *****************************************
changed: [localhost]
TASK [nginx_role : Copy index.html] ***************************************
changed: [localhost]
TASK [nginx_role : Start and enable NGINX service] ************************
changed: [localhost]
PLAY RECAP ****************************************************************
localhost : ok=4  changed=3  unreachable=0  failed=0  skipped=0  rescued=0  ignored=0
```

You can then verify by opening a browser and visiting:

```
http://localhost
```

✅ A web page titled **“NGINX deployed using Ansible!”** should appear.

---

### **RESULT:**

The Ansible role and playbook were successfully created and executed, and the web server was deployed with NGINX configuration automatically.
