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

1. Windows (using PowerShell)
Windows 10 and 11 have OpenSSH built-in, making it very similar to Linux.

Generate Key: Open PowerShell and run:

PowerShell
ssh-keygen -t ed25519 -C "windows_devops"
View Public Key: Run this to see the text you need to copy:

PowerShell
    Get-Content $HOME\.ssh\id_ed25519.pub
    ```
*   **Connect:**
    ```powershell
    ssh -i $HOME\.ssh\id_ed25519 ubuntu@<EC2-Public-IP>
    ```

### 2. Ubuntu (Linux)
The terminal is native here, so the commands are direct and handle permissions easily.

*   **Generate Key:** Open your **Terminal** and run:
    
```bash
    ssh-keygen -t ed25519 -C "ubuntu_devops"
    ```
*   **View Public Key:**
    
```bash
    cat ~/.ssh/id_ed25519.pub
    ```
*   **Fix Permissions:** Ensure your local keys are protected:
    
```bash
    chmod 600 ~/.ssh/id_ed25519
    ```
*   **Connect:**
    
```bash
    ssh -i ~/.ssh/id_ed25519 ubuntu@<EC2-Public-IP>
    ```

### 3. macOS
macOS is Unix-based, so the steps are almost identical to Ubuntu but utilize the native **Terminal** or **iTerm2**.

*   **Generate Key:** Run:
    
```zsh
    ssh-keygen -t ed25519 -C "macos_devops"
    ```
*   **View Public Key:**
    
```zsh
    cat ~/.ssh/id_ed25519.pub | pbcopy
    ```
    *(Tip: Adding `| pbcopy` at the end automatically copies it to your clipboard!)*
*   **Connect:**
    
```zsh
    ssh -i ~/.ssh/id_ed25519 ubuntu@<EC2-Public-IP>
    ```

---

### Quick Troubleshooting Tip
Regardless of your OS, if you receive a **"Permission Denied"** or **"Unprotected Private Key File"** error when trying to connect, it means your computer thinks the private key is too "public." 

> **The Fix:** Make sure the folder (`.ssh`) and the key itself (`id_ed25519`) are restricted so only you can read them. On Mac/Ubuntu, this is `chmod 600`. On Windows, you can right-click the file > Properties > Security to limit access to your specific user account.
```</EC2-Public-IP></EC2-Public-IP>
**html-ec2-deploy**

A simple HTML page for CI/CD demo

This guide outlines securing SSH keys in GitHub Secrets and deploying static HTML to AWS EC2 using GitHub Actions workflows. The process involves configuring secrets for host and user data, then implementing a YAML workflow to automate file transfers and Nginx restarts upon pushes to the main branch. The generated documentation covers these essential DevOps steps to establish a functional CI/CD pipeline for web server automation.
