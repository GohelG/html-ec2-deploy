````markdown
# 🚀 HTML EC2 Deploy — CI/CD Pipeline with GitHub Actions & AWS EC2

A simple DevOps project demonstrating automated deployment of a static HTML website to an AWS EC2 instance using GitHub Actions and SSH-based deployment.

This project showcases a basic CI/CD workflow where every push to the `main` branch automatically deploys the latest application changes to an Ubuntu EC2 server running Nginx.

---

# 📌 Project Overview

This repository demonstrates:

- ✅ Automated CI/CD deployment using GitHub Actions
- ✅ Secure SSH authentication using GitHub Secrets
- ✅ Deployment to AWS EC2 Ubuntu instance
- ✅ Static website hosting with Nginx
- ✅ Automatic code updates on every GitHub push

---

# 🏗️ Architecture Workflow

```text
Developer Push → GitHub Repository → GitHub Actions Workflow
        ↓
SSH Connection using Secrets
        ↓
AWS EC2 Instance (Ubuntu + Nginx)
        ↓
Updated Static HTML Website
````

---

# 🛠️ Tech Stack

| Technology     | Purpose           |
| -------------- | ----------------- |
| HTML           | Frontend Web Page |
| GitHub Actions | CI/CD Automation  |
| AWS EC2        | Hosting Server    |
| Ubuntu         | Operating System  |
| Nginx          | Web Server        |
| SSH            | Secure Deployment |

---

# 📂 Project Structure

```text
html-ec2-deploy/
│
├── index.html
├── README.md
└── .github/
    └── workflows/
        └── main.yml
```

---

# ⚙️ Phase 1 — Server Setup on AWS EC2

## Step 1.1 — Launch EC2 Instance

Create an Ubuntu EC2 instance from AWS Console.

### Open Required Ports in Security Group

| Port | Purpose      |
| ---- | ------------ |
| 22   | SSH Access   |
| 80   | HTTP Traffic |

---

## Step 1.2 — Create Deployment User

Connect to EC2:

```bash
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>
```

Create a new deployment user:

```bash
sudo adduser devopsuser
```

(Optional) Grant sudo access:

```bash
sudo usermod -aG sudo devopsuser
```

---

## Step 1.3 — Configure SSH Access

Switch to the new user:

```bash
su - devopsuser
```

Create SSH directory and permissions:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh

touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Paste your local machine public key:

```bash
nano ~/.ssh/authorized_keys
```

---

# 🌐 Phase 2 — Application Setup

Create deployment directory:

```bash
sudo mkdir -p /var/www/html/my-app
sudo chown -R devopsuser:devopsuser /var/www/html/my-app
```

---

## Sample HTML Page

Create an `index.html` file in your repository:

```html
<!DOCTYPE html>
<html>
<head>
    <title>CI/CD Demo</title>
</head>
<body>
    <h1>🚀 Deployment Successful!</h1>
    <p>Managed automatically using GitHub Actions.</p>
</body>
</html>
```

---

# 🔐 Phase 3 — Configure GitHub Secrets

Navigate to:

```text
GitHub Repository → Settings → Secrets and Variables → Actions
```

Add the following repository secrets:

| Secret Name | Description             |
| ----------- | ----------------------- |
| EC2_HOST    | EC2 Public IP           |
| EC2_USER    | devopsuser              |
| EC2_SSH_KEY | Private SSH Key Content |

---

# ⚡ Phase 4 — GitHub Actions Workflow

Create:

```text
.github/workflows/main.yml
```

Add the following workflow:

```yaml
name: EC2 Deployment Pipeline

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Deploy to AWS EC2
        uses: appleboy/ssh-action@v1.0.3

        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          port: 22

          script: |
            cd /var/www/html/my-app

            if [ ! -d ".git" ]; then
              git clone https://github.com/${{ github.repository }}.git .
            else
              git pull origin main
            fi

            sudo systemctl restart nginx
```

---

# ✅ Final Verification

## Push Code to GitHub

```bash
git add .
git commit -m "Initial deployment"
git push origin main
```

---

## Monitor Deployment

Go to:

```text
GitHub Repository → Actions
```

Check workflow execution logs.

---

## Access the Website

Open in browser:

```text
http://<EC2_PUBLIC_IP>/my-app/index.html
```

---

# 🔒 Security Best Practices

* Never hardcode private keys in code
* Store sensitive credentials in GitHub Secrets
* Restrict EC2 Security Group access
* Use SSH key authentication instead of passwords
* Rotate SSH keys periodically

---

# 📈 Future Improvements

* Add custom domain with Cloudflare
* Enable HTTPS using SSL certificates
* Add Docker container deployment
* Integrate Load Balancer & Auto Scaling
* Deploy React + Node.js full-stack applications

---

# 👨‍💻 Author

**Gautam Gohil**

DevOps & Cloud Enthusiast 🚀

```
```
