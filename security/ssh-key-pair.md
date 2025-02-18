# SSH Key Pair - Learning 🗝️

## Where to Store SSH Keys Locally?  
- Always store your **private key pairs** in `~/.ssh/`
- Keep them **secure** and **never share** private keys with anyone.

## About SSH Keys 🔑  
- SSH key pairs consist of a **private key** (kept secret) and a **public key** (shared with remote systems).  
- You can extract a **public key** from a private key using:  
  ```sh
  ssh-keygen -y -f <key-pair.pem>
  ```
- If you get a permission error, update the key permissions:  
  ```sh
  chmod 400 <key-pair.pem>
  ```

## Best Practices ✅  
- **Use different SSH keys** on every device to enhance security.  
- **Never store private keys** in public repositories like GitHub.  
- **Generate the public key** from the private key and add it to the instance.  

---

## Public Key Storage on a Remote Server  
- The public key is stored in the `~/.ssh/authorized_keys` file of the remote instance.  
- When you add a public key to a server, it allows connections from the corresponding private key.  

---

## How Does SSH Authentication Work? 🤝  

Imagine **SSH (Secure Shell)** as a **secret handshake** between you and a server. The **public and private keys** act as special security tokens to verify your identity.

### **Step-by-Step SSH Authentication Process**
1️⃣ **Key Pair Creation**  
   - You generate an SSH key pair, consisting of:  
     - A **public key** (a visible badge).  
     - A **private key** (a secret only you know).  

2️⃣ **Sharing the Public Key**  
   - You place your **public key** on the server (inside `~/.ssh/authorized_keys`).  
   - Your **private key** remains safe on your local system.  

3️⃣ **Initiating Connection**  
   - When you attempt to SSH into the server:  
     ```sh
     ssh -i <key-pair.pem> user@server-ip
     ```
   - Your system tells the server, "I’d like to connect!"  

4️⃣ **The Server Challenges You**  
   - The server sends a **challenge** (an encrypted message).  
   - This challenge can only be answered correctly if you have the **right private key**.  

5️⃣ **Proving Your Identity**  
   - Your SSH client **decrypts the challenge** using your private key.  
   - It sends back the correct response (like a secret handshake move).  

6️⃣ **Server Verifies the Response**  
   - The server **checks your response** using the stored **public key**.  
   - If it matches, the server knows it’s really you.  

7️⃣ **Access Granted!** 🚀  
   - The server allows the SSH session to start, and now you can securely communicate.  

---

### **Analogy: SSH Authentication as a Secret Handshake**
Think of SSH authentication like a **secret handshake** between you and a friend:  
✅ Your **public key** is like a visible badge that tells the server who you are.  
✅ Your **private key** is like a secret handshake move only you know.  
✅ When you try to connect, the server challenges you to perform the handshake.  
✅ If you do it correctly (using your private key), the server recognizes you and lets you in.  

---

## **Key Takeaways**
🔹 Store private keys securely in `~/.ssh/`  
🔹 Public keys are stored in `~/.ssh/authorized_keys` on the server  
🔹 Use `ssh-keygen -y -f <key-pair.pem>` to extract public keys  
🔹 Always **use different keys** for different devices  
🔹 SSH authentication is like a **secure secret handshake**  
