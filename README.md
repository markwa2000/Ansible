# 🚀 Ansible Project: Basic Roles for Nginx & Tomcat

This project contains two simple Ansible roles:

- ✅ **Nginx** – Installs the Nginx web server
- ✅ **Tomcat** – Installs Apache Tomcat with Java, deploys a sample web app

---

## 📁 Project Structure

```
.
├── site.yml                  # Main playbook
├── inventory.ini             # Inventory file with target hosts
└── roles/
    ├── nginx/
    │   └── tasks/
    │       └── main.yml      # Task to install nginx
    └── tomcat/
        ├── tasks/
        │   └── main.yml      # Java, Tomcat install & setup
        └── vars/
            └── main.yml      # Tomcat-related variables
```

---

## 🔧 How to Use

1. **Clone this repo**
2. **Edit your `inventory.ini`** file to include your target host(s)
3. **Run the playbook:**

```bash
ansible-playbook -i inventory.ini site.yml
```

---

## 🧱 Role: Nginx

📄 `roles/nginx/tasks/main.yml`

```yaml
- name: install nginx 
  apt:
    name: nginx
    state: present
```

✅ **What it does:**  
Installs the latest version of **Nginx** using `apt`.

---

## ☕ Role: Tomcat

📄 `roles/tomcat/tasks/main.yml`

This role installs:

- Java (OpenJDK 11)
- Creates a dedicated `tomcat` user and group
- Downloads and extracts Tomcat
- Sets executable permissions
- Starts the Tomcat server
- Installs a sample web app from a zip file

📄 `roles/tomcat/vars/main.yml` (variables used):

```yaml
tomcat_version: "9.0.21"
tomcat_home: "/opt/tomcat"
java_package: "openjdk-11-jdk"
tomcat_user: "tomcat"
tomcat_group: "tomcat"
```

---

## 🌐 Sample Web App

```yaml
- name: Install webapps from web
  get_url:
    url: "https://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/wls/10g/r3/cluster/session_state/files/shoppingcart.zip"
    dest: "/tmp/shoppingcart.zip"

- name: unzip
  ansible.builtin.unarchive:
    src: "/tmp/shoppingcart.zip"
    dest: "/opt/tomcat/webapps/"
    remote_src: yes
```

---

## ⚠️ Notes & Future Improvements

1. 🔒 Enable `tomcat.service` to auto-start on boot
2. 🧪 Add HTTP health checks for Nginx and Tomcat
3. 🔁 Improve idempotence to avoid re-running tasks
4. 🔐 Add firewall or security rules
5. 🧹 Clean up downloaded files after install
6. ⚙️ Move common vars to `defaults/main.yml`

---

## ✅ Tested On

- Ubuntu 20.04/22.04 EC2 instance
- Ansible 2.9+ / 2.12+

---

## 📬 Author

Made by **Vinny Markwa**  
GitHub: [@vinnymarkwa](https://github.com/vinnymarkwa)