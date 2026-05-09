html-ec2-deploy

A simple HTML page for CI/CD demo

This guide outlines securing SSH keys in GitHub Secrets and deploying static HTML to AWS EC2 using GitHub Actions workflows. The process involves configuring secrets for host and user data, then implementing a YAML workflow to automate file transfers and Nginx restarts upon pushes to the main branch. The generated documentation covers these essential DevOps steps to establish a functional CI/CD pipeline for web server automation.


Step 1.1 – Launch an EC2 Instance

Step 1: Launch Instance Wizard
- Log in to your AWS Management Console.
- Search for EC2 in the top search bar and click on the service.
-  On the EC2 Dashboard, click the Launch instance button.

Step 2: Name and OS (AMI)
- Name: In the "Name and tags" field, type Web1.
- Application and OS Images (Amazon Machine Image):
- Select Ubuntu from the Quick Start icons.
Ensure the dropdown shows a recent version (e.g., Ubuntu Server 24.04 LTS or 22.04 LTS).

Step 3: Instance Type and Key Pair
- Instance type: Open the dropdown and select t2.micro. (Note: Make sure you are in a region that supports t3.micro, otherwise t2.micro is the usual free-tier alternative).
- Key pair (login): Select an existing key pair or click Create new key pair so you can SSH into the box later. You'll need this .pem or .ppk file.

Step 4: Network Settings (Security Group)
- This is where we handle your port access. In the Network settings section, click Edit (top right of the section).
- Security group: Select Create security group.
- Security group name: Give it a name like web-server-sg.
- Inbound Security Groups Rules:
- Rule 1 (SSH):
  - Type: SSH
  - Protocol: TCP
  - Port range: 22
- Source type: Select My IP (Best practice for security) or Anywhere (0.0.0.0/0).
  - Rule 2 (HTTP):
  - Click Add security group rule.
  - Type: HTTP
  - Protocol: TCP
  - Port range: 80
  -  Source type: Anywhere (0.0.0.0/0).

Step 5: Final Review and Launch
- Configure storage: The default (8GB or 30GB GP3) is usually fine for a basic Ubuntu server.
- Summary: Double-check the panel on the right to ensure it says Web1, Ubuntu, and t3.micro.
- Click Launch instance.

# Example command to connect to your EC2 instance
ssh -i DevOpsGA.pem ubuntu@<EC2-PUBLIC-IP>

Step 1.2 – Create a DevOps User for Deployment