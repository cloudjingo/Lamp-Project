# 🚀 LAMP Project with Ansible on AWS

## 📌 Project Overview

This project provisions and configures a LAMP (Linux, Apache, MySQL, PHP) stack across AWS EC2 instances using Ansible. It automates:

* User creation
* EC2 instance provisioning
* Server configuration
* Software installation
* Firewall and optional SSL setup
* Cleanup of AWS resources

---

## 🗂️ Project Structure

```bash
ansible-aws-project/
├── ansible.cfg
├── inventory.ini
├── create_ec2_instances.yml
├── configure-ubuntu-db.yml
├── configure-amazon-web.yml
├── cleanup.yml
├── README.md
└── diagrams/
    └── architecture.png
```

---

## 🖥️ EC2 Instance Roles

| Instance        | OS             | Role           | Services    |
| --------------- | -------------- | -------------- | ----------- |
| Web Server      | Amazon Linux 2 | Apache (httpd) | Web Hosting |
| Database Server | Ubuntu 22.04   | MySQL Server   | DB Storage  |

---

## ⚙️ Prerequisites

* ✅ AWS CLI configured with credentials
* ✅ Ansible & Python 3.8+ installed
* ✅ SSH key pair with access to EC2
* ✅ Git installed on your local system

---

## 🌩️ Provision EC2 Instances

Update `create_ec2_instances.yml` with:

* AMI IDs
* Instance types
* Region and key names
* Security group settings

Then run:

```bash
ansible-playbook create_ec2_instances.yml
```

---

## 📝 Update Inventory

Update `inventory.ini` with the EC2 public IPs:

```ini
[web]
3.88.216.51 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/Bolaji-keypair.pem

[db]
34.224.71.113 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/Bolaji-keypair.pem
```

---

## ⚙️ Configure Servers

### 🧩 Web Server Setup (Apache):

```bash
ansible-playbook -i inventory.ini configure-amazon-web.yml
```

### 🗃️ DB Server Setup (MySQL):

```bash
ansible-playbook -i inventory.ini configure-ubuntu-db.yml
```

---

## ✅ Verification Checklist

### 🌐 Web Server:

* Visit: `http://<web-server-ip>`
* Apache welcome page should load

### 🛢️ DB Server:

SSH into the server:

```bash
ssh -i ~/.ssh/Bolaji-keypair.pem ubuntu@<db-server-ip>
sudo systemctl status mysql
```

* MySQL service should be `active (running)`

---

## 🔐 Additional Security Tasks

### 🔥 Enable UFW on DB Server:

Add to your DB playbook:

```yaml
- name: Enable UFW
  ufw:
    state: enabled
    policy: allow
```

### 🔓 Open AWS Security Group Ports:

Ensure the following ports are open:

* 22 (SSH)
* 80 (HTTP)
* 3306 (MySQL – allow only internal)

---

## 🧼 Cleanup Playbook

To terminate EC2 instances:

```yaml
- name: Terminate EC2 Instances
  hosts: localhost
  gather_facts: no
  tasks:
    - name: Terminate EC2
      ec2:
        state: absent
        instance_ids:
          - i-xxxxxxxxxxxxxxxxx
          - i-yyyyyyyyyyyyyyyyy
        region: us-east-1
        aws_access_key: YOUR_KEY
        aws_secret_key: YOUR_SECRET
```

Run with:

```bash
ansible-playbook cleanup.yml
```

---

## 🖼️ Architecture Diagram

![LAMP Architecture](diagrams/architecture.png)

---

## 📤 Push to GitHub

```bash
git init
git add .
git commit -m "Initial LAMP AWS Ansible Project"
git branch -M main
git remote add origin https://github.com/cloudjingo/Lamp-Project.git
git push -u origin main
```

---

## 💻 Technologies Used

* ✅ Ansible
* ✅ AWS EC2
* ✅ Apache HTTP Server
* ✅ MySQL Server
* ✅ Ubuntu 22.04 / Amazon Linux 2
* ✅ Git & GitHub

---

## 📄 License

MIT License — free to use and modify

---

## 👤 Author

**Bolaji** ([@cloudjingo](https://github.com/cloudjingo))

---


