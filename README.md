html-ec2-deploy

A simple HTML page for CI/CD demo

This guide outlines securing SSH keys in GitHub Secrets and deploying static HTML to AWS EC2 using GitHub Actions workflows. The process involves configuring secrets for host and user data, then implementing a YAML workflow to automate file transfers and Nginx restarts upon pushes to the main branch. The generated documentation covers these essential DevOps steps to establish a functional CI/CD pipeline for web server automation.

Phase 1: Server-Side Preparation
Before GitHub can deploy, the devopsuser must be configured on the Ubuntu instance to handle incoming SSH connections.

Step 1.2 – Create a DevOps User
Log into your EC2 instance as the default ubuntu user.

Create the new user:

Bash
sudo adduser devopsuser
Grant the user administrative privileges (optional but helpful for deployments):

Bash
sudo usermod -aG sudo devopsuser
Step 1.3 – Configure SSH Key for devopsuser
Switch to the new user:

Bash
   su - devopsuser
Create the SSH directory and set permissions:

Bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   touch ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
Paste your Public Key (generated locally) into the authorized_keys file using nano ~/.ssh/authorized_keys.

Phase 2: Application Setup (Simple HTML Page)
To verify the deployment works, create a target directory on the server where the code will live.

Create a project folder:

Bash
   sudo mkdir -p /var/www/html/my-app
   sudo chown -R devopsuser:devopsuser /var/www/html/my-app
On your local machine, create a simple index.html file in your GitHub repository:

HTML
   <!DOCTYPE html>
   <html>
   <body>
       <h1>Deployment Successful!</h1>
       <p>Managed via GitHub Actions.</p>
   </body>
   </html>
Phase 3: Storing Secrets & Workflow
Step 3.1: Store SSH Key in GitHub Secrets
Navigate to your repository on GitHub.

Go to Settings > Secrets and variables > Actions.

Click New repository secret for each of the following:

EC2_HOST: Your EC2 Public IP (e.g., 54.123.45.67).

EC2_USER: devopsuser.

EC2_SSH_KEY: The entire content of your Private Key (usually found at ~/.ssh/id_ed25519 on your local machine).

Step 3.2: Create GitHub Actions Workflow
Create a file at .github/workflows/main.yml in your repository and paste the following:

YAML
name: EC2 Deployment

on:
  push:
    branches:
      - main  # Triggers deployment on push to main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          port: 22
          script: |
            # Navigate to the app directory
            cd /var/www/html/my-app
            # Initialize git if not already done, or pull latest changes
            if [ ! -d ".git" ]; then
              git clone https://github.com/${{ github.repository }}.git .
            else
              git pull origin main
            fi
Final Verification
Commit and Push: Push these changes to your GitHub main branch.

Monitor: Go to the Actions tab in GitHub to watch the progress.

Check Web1: Navigate to http://<EC2-PUBLIC-IP>/my-app/index.html in your browser.