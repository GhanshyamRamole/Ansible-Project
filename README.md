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
