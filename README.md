# 🚀 Odoo 17 Deployment on Azure VM

This project demonstrates a complete, production-ready deployment of Odoo Community Edition on a cloud-based virtual machine.

It includes backend setup, web server configuration, and service management—similar to real client requirements in freelancing and DevOps roles.

---

## 📌 Project Overview

* Installed Odoo 17 (Community Edition)
* Configured PostgreSQL database
* Set up Python virtual environment
* Configured Nginx as reverse proxy
* Enabled systemd service (auto-start)
* Activated core business modules
* Enabled PDF report generation

---

## 🛠️ Tech Stack

* Ubuntu 24.04 (Azure VM)
* Python 3.12
* PostgreSQL
* Nginx
* Odoo 17

---

## ⚙️ Deployment Steps

### 1. Update System

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Install Dependencies

```bash
bash setup/install_dependencies.sh
```

---

### 3. Install PostgreSQL

```bash
sudo apt install postgresql postgresql-contrib -y
sudo -u postgres createuser -s odoo
```

---

### 4. Clone Odoo

```bash
sudo mkdir /opt/odoo
sudo chown $USER:$USER /opt/odoo

git clone https://github.com/odoo/odoo --depth 1 --branch 17.0 /opt/odoo
```

---

### 5. Setup Virtual Environment

```bash
cd /opt/odoo
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

### 6. Configure Odoo

```bash
sudo nano /etc/odoo.conf
```

Paste content from:

```
setup/odoo_config.conf
```

---

### 7. Fix PostgreSQL Authentication

```bash
sudo nano /etc/postgresql/*/main/pg_hba.conf
```

Change authentication method:

```
peer → md5
```

Restart:

```bash
sudo systemctl restart postgresql
```

---

### 8. Run Odoo

```bash
cd /opt/odoo
source venv/bin/activate
python odoo-bin -c /etc/odoo.conf
```

Access:

```
http://<public-ip>:8069
```

---

### 9. Configure Nginx

```bash
sudo nano /etc/nginx/sites-available/odoo
```

Paste content from:

```
setup/nginx_config.conf
```

Enable and restart:

```bash
sudo ln -s /etc/nginx/sites-available/odoo /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```

---

### 10. Enable PDF Reports

```bash
sudo apt install wkhtmltopdf -y
```

---

### 11. Create Odoo Service

```bash
sudo nano /etc/systemd/system/odoo.service
```

Add service configuration, then run:

```bash
sudo systemctl daemon-reload
sudo systemctl start odoo
sudo systemctl enable odoo
```

---

## 🌐 Final Access

```
http://<your-public-ip>
```

---

## 📸 Screenshots

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0f28f9de-392a-480c-8957-4e576737e619" />



<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/336beac0-2c32-4524-91d6-d67a57494dbc" />



## 🎯 Key Learnings

* End-to-end Odoo deployment on cloud VM
* PostgreSQL configuration and authentication handling
* Nginx reverse proxy setup
* Debugging real-world deployment issues
* Running applications as system services

---

## ⚠️ Note

Odoo source code is not included in this repository.
Clone it from the official GitHub repository during setup.

---

## 👨‍💻 Author

Ajay Kumar
DevOps Engineer
