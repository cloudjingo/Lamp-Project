<<<<<<< HEAD
# LAMP Project with Ansible on AWS

## 📌 Project Overview

This project provisions and configures a simple LAMP (Linux, Apache, MySQL, PHP) stack across AWS EC2 instances using Ansible. It automates user creation, server setup, software installation, and optionally includes cleanup and security tasks.

---

## 🗂️ Directory Structure
=======

# Bolaji Week 1 Project – AWS EC2 + Ansible Automation

## 📘 Project Overview

This project provisions AWS EC2 instances and configures them as:
- A **web server** (Amazon Linux 2 with Apache)
- A **database server** (Ubuntu with PostgreSQL)

Automation is performed using **Ansible** for provisioning, configuration, and verification.

---

## 🖥️ Architecture Diagram

```
 [Local Machine (Ansible Control Node)]
            |
            | SSH + Ansible
            |
      ---------------------
      |                   |
[Amazon Linux 2]     [Ubuntu 22.04]
   Web Server          DB Server
   Apache              PostgreSQL
```

---

## 🛠️ Tools & Technologies

- **AWS EC2** (Ubuntu 22.04, Amazon Linux 2)
- **Ansible**
- **SSH Key Pair**
- **Bash Scripting**
- **YAML**
- **PostgreSQL**
- **Apache (httpd)**

---

## 📁 Project Structure
>>>>>>> b4dc2eaba888320749b9039bd0811faecc046a8d

```
ansible-aws-project/
├── ansible.cfg
├── inventory.ini
├── create_ec2_instances.yml
<<<<<<< HEAD
├── configure-ubuntu-db.yml
├── configure-amazon-web.yml
├── cleanup.yml
├── README.md
└── diagrams/
    └── architecture.png
=======
├── configure-amazon-web.yml
├── configure-ubuntu-db.yml
├── cleanup.yml (optional)
└── README.md
>>>>>>> b4dc2eaba888320749b9039bd0811faecc046a8d
```

---

<<<<<<< HEAD
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
=======
## 🚀 Project Steps

### Step 1: Prepare Your Environment

```bash
python3 -m venv ansible-env
source ansible-env/bin/activate
pip install ansible boto3 botocore
```

### Step 2: SSH Key Setup

Upload your `.pem` key (e.g., `Bolaji-keypair.pem`) to `~/.ssh/`:

```bash
chmod 400 ~/.ssh/Bolaji-keypair.pem
```

Update `inventory.ini` with IPs and key:

```ini
[web]
<web-server-ip> ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/Bolaji-keypair.pem

[db]
<db-server-ip> ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/Bolaji-keypair.pem
```

### Step 3: Launch EC2 Instances
>>>>>>> b4dc2eaba888320749b9039bd0811faecc046a8d

```bash
ansible-playbook create_ec2_instances.yml
```

<<<<<<< HEAD
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
=======
### Step 4: Configure Web and DB Servers

```bash
ansible-playbook -i inventory.ini configure-amazon-web.yml
ansible-playbook -i inventory.ini configure-ubuntu-db.yml
```

### Step 5: Validate

- Web server: Open `http://<web-server-ip>` in a browser.
- DB server: SSH into Ubuntu and check PostgreSQL service:

```bash
sudo systemctl status postgresql
```

---

## 🧪 Sample Ansible Tasks

### Configure Web Server (`configure-amazon-web.yml`)

```yaml
- name: Configure Amazon Linux 2 as a web server
  hosts: web
  become: yes
  tasks:
    - name: Install Apache
      ansible.builtin.yum:
        name: httpd
        state: present

    - name: Start and enable Apache
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: true
```

### Configure DB Server (`configure-ubuntu-db.yml`)

```yaml
- name: Configure Ubuntu as a PostgreSQL DB server
  hosts: db
  become: yes
  tasks:
    - name: Install PostgreSQL
      ansible.builtin.apt:
        name: postgresql
        state: present
        update_cache: yes

    - name: Ensure PostgreSQL is running
      ansible.builtin.service:
        name: postgresql
        state: started
        enabled: true
```

---

## 🔐 Extra Tasks (Optional Enhancements)

- Open port 80 and 5432 in AWS Security Groups.
- Add firewall rules using Ansible.
- Add SSL (e.g., Let’s Encrypt with Certbot).
- Setup cron jobs or log monitoring.

---

## 🧹 Cleanup

```yaml
- name: Terminate EC2 instances
  hosts: localhost
  connection: local
  gather_facts: false
  tasks:
    - name: Terminate instances
      ec2_instance:
        state: absent
        region: us-east-1
        instance_ids:
          - i-xxxxxxxxxxxxxxxxx
          - i-yyyyyyyyyyyyyyyyy
>>>>>>> b4dc2eaba888320749b9039bd0811faecc046a8d
```

---

<<<<<<< HEAD
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
=======
## ✅ Status

✅ Web server works: Apache verified via public IP  
✅ PostgreSQL server runs and listens  
✅ Ansible automates provisioning, config, and verification

---

## 📦 Future Enhancements

- Add Load Balancer (ELB)
- Auto-scale EC2 instances
- Use Ansible roles for modular code
- Add monitoring (CloudWatch, Ansible callbacks)
>>>>>>> b4dc2eaba888320749b9039bd0811faecc046a8d
