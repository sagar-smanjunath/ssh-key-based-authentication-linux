# ssh-key-based-authentication-linux
Implemented SSH key-based authentication to enable secure, passwordless login and remote command execution between Linux servers.

📌 Project Overview
       -This project demonstrates how to configure passwordless SSH authentication between two Linux servers using SSH key pairs.

💡 Scenario
Two servers:
Server1 (Client) → 192.168.174.xxx
Server2 (Target) → 192.168.174.132
A user (feb15) wants to connect from Server1 → Server2 without entering a password

👉 Solution: Use SSH key-based authentication

⚙️ Technologies Used
Linux (RHEL/CentOS)
SSH Protocol
Key-based Authentication

🏗️ Architecture

Server1 (Client)                  Server2 (Remote)
User: feb15                       User: feb15
   │                                   │
   │ ---- SSH (Public Key) ----------> │
   │                                   │
   │ <---- Passwordless Access ------- │
   
🚀 Step-by-Step Configuration

🔹 1. Create User on Both Servers
useradd feb15
passwd feb15
🔹 2. Test Normal SSH Login (Password-Based)

From Server1:

su - feb15
ssh feb15@192.168.174.132
First time → asks: Are you sure? → type yes
Then → asks for password
🔹 3. Generate SSH Key Pair (Server1)
ssh-keygen -t rsa

👉 This creates:

id_rsa → Private Key 🔒 (DO NOT SHARE)
id_rsa.pub → Public Key 📤

Verify:

cd ~/.ssh
ls
🔹 4. Copy Public Key to Server2
scp ~/.ssh/id_rsa.pub feb15@192.168.174.132:~
🔹 5. Configure SSH on Server2

Login to Server2:

su - feb15

Create SSH directory:

mkdir ~/.ssh
chmod 700 ~/.ssh

Move public key:

cp id_rsa.pub ~/.ssh/
cd ~/.ssh
mv id_rsa.pub authorized_keys
chmod 640 authorized_keys
🔹 6. Test Passwordless Login

From Server1:

ssh feb15@192.168.174.132

✅ Login should happen without password

🔹 7. Execute Remote Commands
ssh feb15@192.168.174.132 "uptime; uname; ifconfig"

👉 Commands run directly on remote server without login prompt

📸 Screenshots Explanation 

📸 Image 1: Architecture of SSH Passwordless Authentication

Illustrates communication between Server1 (client) and Server2 (remote)
Shows SSH key-based authentication flow
Includes key generation and public key transfer process

📸 Image 2: SSH Key Pair Generation

Command used: ssh-keygen -t rsa
Generates two keys:
id_rsa → Private Key (kept secure)
id_rsa.pub → Public Key (shared with remote server)

📸 Image 3: .ssh Directory Structure

Displays contents of .ssh directory
Confirms presence of:
id_rsa (private key)
id_rsa.pub (public key)

📸 Image 4: Public Key Transfer using SCP

Command used:

scp id_rsa.pub feb15@192.168.174.132:~
Public key successfully copied to remote server

📸 Image 5: Configuring authorized_keys on Target Server

Public key moved and renamed to authorized_keys
Ensures passwordless authentication
Required permissions applied:
.ssh → 700
authorized_keys → 640

📸 Image 6: Passwordless Login & Remote Command Execution

SSH login performed without password

Example command:

ssh feb15@192.168.174.132 "uptime; uname; ifconfig"
Demonstrates successful remote execution without authentication prompt
✅ Key Features

-Eliminates password-based login
-Secure authentication using SSH keys
-Faster remote access
-Enables automation (scripts, deployments)

⚠️ Important Notes

-Never share private key (id_rsa)
-Always set proper permissions:
-.ssh → 700
-authorized_keys → 640
-Ensure SSH service is running

🎯 Advantages

-Access multiple servers without password
-Useful for automation & DevOps
-Improves security over password login

📌 Conclusion

SSH key-based authentication provides a secure and efficient way to access remote systems without passwords, making it ideal for system administration and automation tasks.
