
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

```
ansible-aws-project/
├── ansible.cfg
├── inventory.ini
├── create_ec2_instances.yml
├── configure-amazon-web.yml
├── configure-ubuntu-db.yml
├── cleanup.yml (optional)
└── README.md
```

---

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

```bash
ansible-playbook create_ec2_instances.yml
```

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
```

---

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
