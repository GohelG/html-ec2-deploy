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