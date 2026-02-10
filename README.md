# openclaw

azureuser@openclaw:~$ sudo useradd openclaw

azureuser@openclaw:~$ sudo passwd openclaw 
New password: 
Retype new password: 
passwd: password updated successfully
azureuser@openclaw:~$ sudo usermod -aG sudo openclaw
azureuser@openclaw:~$ id openclaw
uid=1001(openclaw) gid=1001(openclaw) groups=1001(openclaw),27(sudo)

azureuser@openclaw:~$ sudo mkdir -p /home/openclaw/.ssh 
azureuser@openclaw:~$ sudo cp -rp /home/azureuser/.ssh/authorized_keys /home/openclaw/.ssh 
azureuser@openclaw:~$ sudo chown -R openclaw:openclaw /home/openclaw/.ssh 
azureuser@openclaw:~$ sudo chmod 700 /home/openclaw/.ssh
azureuser@openclaw:~$ sudo chmod 600 /home/openclaw/.ssh/authorized_keys

azureuser@openclaw:~$ sudo ufw default deny incoming 
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)
azureuser@openclaw:~$ sudo ufw default allow outgoing 
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
azureuser@openclaw:~$ sudo ufw allow 22/tcp
Rules updated
Rules updated (v6)
azureuser@openclaw:~$ sudo ufw allow 2222/tcp
Rules updated
Rules updated (v6)
azureuser@openclaw:~$ sudo ufw enable 
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup

 ~/Downloads   main ● ?  ssh -i openclaw_key.pem openclaw@20.40.208.164                                                                    ✔  10271  10:32:16 
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1017-azure x86_64)

openclaw@openclaw:~$ sudo vi /etc/ssh/sshd_config
[sudo] password for openclaw: 

chnage the below values and uncomment 

#change Port from 22 To 2222

PORT 2222

# Permit Root Login to NO

PermitRootLogin no

#PasswordAuthentication from yes To no

PasswordAuthentication no

openclaw@openclaw:~$ sudo systemctl stop ssh.socket
openclaw@openclaw:~$ sudo systemctl restart ssh
openclaw@openclaw:~$ sudo ss -tulpn | grep ssh 
tcp   LISTEN 0      4096           0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=7372,fd=3),("systemd",pid=1,fd=69))
tcp   LISTEN 0      4096              [::]:22           [::]:*    users:(("sshd",pid=7372,fd=4),("systemd",pid=1,fd=79))

# Disable Systemd Socket Activation (CRITICAL)  Ubuntu 24.04 uses socket activation which holds Port 22 open regardless of what you put in sshd_config you must disable.

$ sudo systemctl disable ssh.socket 
[sudo] password for openclaw: 
Removed "/etc/systemd/system/ssh.service.requires/ssh.socket".
Removed "/etc/systemd/system/sockets.target.wants/ssh.socket".

$ sudo systemctl restart ssh

openclaw@openclaw:~$ sudo ss -tulpn | grep ssh 
tcp   LISTEN 0      128            0.0.0.0:2222      0.0.0.0:*    users:(("sshd",pid=7633,fd=3))            
tcp   LISTEN 0      128               [::]:2222         [::]:*    users:(("sshd",pid=7633,fd=4))    

ssh -p 2222 -i openclaw_key.pem openclaw@20.40.208.164                                                            ✔  10274  10:57:43 
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1017-azure x86_64)
