
# 🚀 Setting Up a Web Server with HTTPD and Podman (Full Guide)

## 🌐 HTTPD Web Server Setup

### 📥 Step 1: Installation
```bash
dnf install httpd
```

### 🚦 Step 2: Starting and Enabling HTTPD
```bash
systemctl start httpd
systemctl enable httpd
```

### 🛠️ Step 3: HTTPD Configuration File
Edit the configuration file:
```bash
vim /etc/httpd/conf/httpd.config
```
Modify the settings:
- `Listen 80`
- `DocumentRoot /var/www/html`

### 📝 Step 4: Creating a Web Page
Create a simple test web page:
```bash
echo "list serveur web" > /var/www/html/test
```

### 🔄 Step 5: Restart HTTPD Service
```bash
systemctl restart httpd
```

### ✅ Step 6: Testing the Web Server
```bash
curl localhost/test
```

---

## 🛡️ Configuring SELinux for Custom Ports

### 🔒 Modifying Default HTTP Port
Example changing port 80 to 87:
```bash
semanage port -a -t http_port_t -p tcp 87
```

### 🔄 Restart HTTPD Service
```bash
systemctl restart httpd
```

### ✅ Testing New Port
```bash
curl localhost:87/test
```

---

## 🔥 Configuring the Firewall

Allow traffic on port 87:
```bash
firewall-cmd --add-port=87/tcp --permanent
firewall-cmd --reload
systemctl restart httpd
```

---

## 📦 Gestion des Conteneurs avec Podman

### 📥 Step 1: Installation
```bash
dnf install crun podman
```

### 🚦 Step 2: Activer et démarrer Podman
```bash
systemctl start podman
systemctl enable podman
```

### 👤 Step 3: Créer un utilisateur (student)
```bash
useradd student
passwd student
```

### 🔑 Step 4: Gestion des conteneurs (rootless mode)
```bash
ssh student@localhost
```

---

## 🗃️ Registry Configuration & Podman Commands

### 🔍 Registry Configuration
File location:
```bash
/etc/containers/registries.conf
```
Common registries:
- `redhat.redhat.io`
- `registry.access.redhat.com`
- `docker.io`

Login example:
```bash
podman login registry.redhat.io
```

### 🐋 Podman Essential Commands

- Search for images:
```bash
podman search httpd
```

- Pull an image:
```bash
podman pull docker.io/centos/httpd-24-centos7:latest
```

- List images:
```bash
podman images
```

- Inspect an image:
```bash
podman inspect <image_id>
```

- Create and run containers:
```bash
podman run -d --name web <image_id>
```

- List containers:
```bash
podman ps             # Running containers
podman ps -a          # All containers
```

- Remove container:
```bash
podman stop <container_id>
podman rm <container_id>
```

- Access container shell:
```bash
podman exec -it web /bin/bash
```

- Create a file in container:
```bash
echo "Test conteneur" > /var/www/html/test
```

- Test container internally:
```bash
curl localhost:8080/test
```

### 🌍 Accessing Containers Externally

Expose port externally:
```bash
podman run -d -p 4200:8080 --name web <image_id>
```

Mounting a volume:
```bash
podman run -d -p 4300:8080 -v /home/student/volume/:/var/www/html/:Z --name web2 <image_id>
```

