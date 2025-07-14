# 📦 Automated Nginx Deployment on AWS EC2 using Ansible

## 📘 Project Overview

This project demonstrates how to automate the **deployment and configuration of the Nginx web server** across multiple AWS EC2 instances using **Ansible**. The automation includes installing, starting, and configuring Nginx on both development and production servers.

### 🔧 Project Architecture

- **Total Instances:** 4 EC2 instances
  - 🧑‍✈️ 1 Master (Ansible control node)
  - 💻 2 Dev Servers: `server-1`, `server-2` (Nginx installed & running)
  - 🏭 1 Production Server: `server-3` (Nginx serving custom static web page)

Ansible uses an **agentless architecture**, meaning no software needs to be installed on target servers. It operates over SSH using YAML-based **playbooks** for automation.

---

## ⚙️ Tools & Technologies Used

| Tool        | Purpose                                       |
|-------------|-----------------------------------------------|
| **AWS EC2** | Infrastructure provisioning (virtual servers) |
| **Ansible** | Automation and configuration management       |
| **SSH/SCP** | Secure communication and file transfer        |
| **Nginx**   | Web server installation & static site hosting |

---

## 🚀 Step-by-Step Implementation

### ✅ Step 1: EC2 Initialization

Launch 4 EC2 instances using the same key-pair:

- `ansible-master-server`
- `server-1`, `server-2` (Dev)
- `server-3` (Production)

### ✅ Step 2: Install Ansible on Master Node

SSH into your Ansible master server and run:

```bash
sudo apt update
sudo apt install ansible -y
```
---

### ✅ Step 3: Transfer PEM Key to Master
Securely move your PEM key to the Ansible master using:

```bash
scp -i "ansible-master-key.pem" ansible-master-key.pem ubuntu@<master-ip>:/home/ubuntu/keys/

````
---
### ✅ Step 4: Configure Ansible Inventory File
Edit the /etc/ansible/hosts file:
```bash
[dev-env]
server-1 ansible_host=<IP> ansible_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-master-key.pem
server-2 ansible_host=<IP> ansible_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-master-key.pem

[production-env]
server-3 ansible_host=<IP> ansible_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-master-key.pem

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```
---
✅ Step 5: Test Server Connections
Check if the control node can ping all servers:
```bash
ansible all -m ping
```
---

### ✅ Step 6: Environment Grouping
dev-env → server-1, server-2

production-env → server-3

This allows targeted playbook execution.
---

### ✅ Step 7: Update All Target Servers
Run an ad-hoc command to update all servers:
```bash
ansible all -a "sudo apt update"
```
---
### 📄 Playbooks
### 📁 installation.yml (For Dev Servers)
```bash
---
- hosts: dev-env
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start Nginx
      service:
        name: nginx
        state: started
        enabled: yes
```
---
### 📁 prod-deployment.yml (For Production Server)
```bash
---
- hosts: production-env
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Copy custom index.html
      copy:
        src: ./index.html
        dest: /var/www/html/index.html

    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```
---

### ✅ Dev Server Deployment
Run the playbook for Dev:
```bash
ansible-playbook installation.yml
server-1 and server-2 will have Nginx installed and running.
```
---
### ✅ Production Deployment
Run the playbook for Production:
```bash
ansible-playbook prod-deployment.yml
```
Access static page at: http://<server-3-public-ip>

### 🧪 Final Verification
### ✅ Nginx running on all dev and prod servers

### ✅ Static ERP website loaded from server-3

### 🎯 Project Outcome
### ⚙️ Zero-touch deployment across EC2 instances

### 🌍 ERP web page served on production

### 📁 Environment separation via Ansible inventory grouping
---
###📘 Key Learnings
Ansible simplifies infrastructure management with agentless architecture

Grouped inventories allow modular playbooks

Automating EC2 configuration is efficient for multi-server environments
---

### 🏁 Conclusion
This project showcases the strength of Ansible + AWS to automate and manage cloud infrastructure at scale. With just a few playbooks and proper grouping, you can:

Reduce manual overhead

Achieve consistency across environments

Rapidly deploy web applications like Nginx


---
