# openclaw



# OpenClaw Server Setup Guide

This document outlines the steps to configure a new user, secure SSH access, and set up firewall rules on Ubuntu 24.04.3 LTS for the OpenClaw server.

---

## 1. Create a new user

```bash
sudo useradd openclaw
sudo passwd openclaw  # Set password
sudo usermod -aG sudo openclaw
id openclaw  # Verify user and groups

2. Configure SSH Key Authentication

sudo mkdir -p /home/openclaw/.ssh
sudo cp -rp /home/azureuser/.ssh/authorized_keys /home/openclaw/.ssh
sudo chown -R openclaw:openclaw /home/openclaw/.ssh
sudo chmod 700 /home/openclaw/.ssh
sudo chmod 600 /home/openclaw/.ssh/authorized_keys

3. Configure Firewall (UFW)

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp      # Optional if legacy port needed temporarily
sudo ufw allow 2222/tcp    # New SSH port
sudo ufw enable


4. Harden SSH Configuration
Edit /etc/ssh/sshd_config:

# Change default SSH port
Port 2222

# Disable root login
PermitRootLogin no

# Disable password authentication
PasswordAuthentication no

Restart ssh

sudo systemctl stop ssh.socket          # Stop socket activation
sudo systemctl disable ssh.socket       # Disable socket activation
sudo systemctl restart ssh
sudo ss -tulpn | grep ssh               # Verify SSH is listening on port 2222

5. Connect to the Server

ssh -p 2222 -i openclaw_key.pem openclaw@<SERVER_IP>

Notes
Critical: On Ubuntu 24.04, systemd socket activation can keep port 22 open. Ensure ssh.socket is disabled.



  
