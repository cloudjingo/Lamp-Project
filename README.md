# LAMP Project with Ansible on AWS

## 📌 Project Overview

This project provisions and configures a simple LAMP (Linux, Apache, MySQL, PHP) stack across AWS EC2 instances using Ansible. It automates user creation, server setup, software installation, and optionally includes cleanup and security tasks.

---

## 🗂️ Directory Structure

```
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

* AWS CLI configured
* Ansible and Python 3.8+ installed
* SSH keypair and access to EC2
* Git

---

## 🚀 Provision EC2 Instances

Edit `create_ec2_instances.yml` with your AMIs and keypair.

```bash
ansible-playbook create_ec2_instances.yml
```

---

## 🔌 Update Inventory File

Edit `inventory.ini` with the public IPs of your EC2 instances.

```
[web]
3.88.216.51 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/Bolaji-keypair.pem

[db]
34.224.71.113 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/Bolaji-keypair.pem
```

---

## 🛠️ Run Playbooks

### Configure Web Server:

```bash
ansible-playbook -i inventory.ini configure-amazon-web.yml
```

### Configure DB Server:

```bash
ansible-playbook -i inventory.ini configure-ubuntu-db.yml
```

---

## ✅ Verification Steps

### Web Server:

* Visit: http\://<web-server-public-ip>
* Should show Apache welcome page or app index

### DB Server:

```bash
sudo systemctl status mysql
```

* Service should be `active (running)`

---

## 🔐 Extra Tasks

### Enable UFW on Ubuntu DB Server:

```yaml
- name: Enable UFW
  ufw:
    state: enabled
    policy: allow
```

### Open Port 80 for HTTP on Web Server:

Ensure AWS security group allows port 80 (HTTP) and 22 (SSH)

---

## 🧹 Cleanup Playbook

```yaml
# cleanup.yml
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

---

## 🖼️ Architecture Diagram

![Architecture Diagram](diagrams/architecture.png)

---

## 📤 Pushing to GitHub

```bash
git init
git add .
git commit -m "Initial LAMP AWS Ansible Project"
git branch -M main
git remote add origin https://github.com/cloudjingo/Lamp-Project.git
git push -u origin main
```

---

## 📚 Technologies Used

* Ansible
* AWS EC2
* Apache
* MySQL
* Ubuntu / Amazon Linux
* GitHub

---

## 📎 License

MIT License - feel free to use and modify.

---

## 🙌 Author

Bolaji (cloudjingo)

---

## ✅ Next Steps

* Add SSL/TLS using Let's Encrypt
* Set up CI/CD pipeline
* Use Ansible roles for modularization
* Export logs to CloudWatch

---
