## Question  
Can you restore a lost PEM file? If not, how can you still access the EC2 instance?

### 📝 Short Explanation  
This question evaluates your knowledge of secure SSH access, key-based authentication, and how to regain access to EC2 instances when your only login method (PEM file) is lost.

## ✅ Answer  
No, you **cannot restore** a lost PEM file — it’s not stored on AWS or recoverable.  
However, you can **regain access** by using a workaround: create a new key pair, attach it to the instance via a temporary EC2 rescue process, and restore SSH access.

```
- We can try by AWS session manager or cloud instance connect i,e. EC2 instance connect.

- Through ssh-keygen generate public and private keys, download private key to local and provide right permissions like chmod 400.

```
# Lab Tasks

```sh
# Select EC2 Instance connect --> we will get access to the instance terminal.

ssh-keygen -t rsa
cd ~ /.ssh
ls -ltr # will have id_rsa and id_rsa.pub 
vim authorized_keys # copy to id_rsa.pub to authorized_keys and download id_rsa(Private key) to local

```

### 📘 Detailed Explanation  

PEM files (private keys) are **never retrievable from AWS** after initial creation. Losing the PEM file means you cannot SSH into the EC2 instance using the existing key pair.

But here’s how you can still regain access:

---

### ✅ Recovery Steps:

#### Step 1: Create a new key pair
```bash
aws ec2 create-key-pair --key-name new-key --query 'KeyMaterial' --output text > new-key.pem
chmod 400 new-key.pem
```

---

#### Step 2: Stop the affected instance  
In the AWS Console or CLI:
```bash
aws ec2 stop-instances --instance-ids i-xxxxxxxxxxxxxxx
```

---

#### Step 3: Detach the root EBS volume from the stopped instance

- Go to **EC2 > Volumes**
- Find the volume attached to your instance (usually `/dev/xvda`)
- **Detach** it

---

#### Step 4: Attach the volume to a temporary (working) instance  
- Attach it as a secondary volume (e.g., `/dev/sdf`)

---

#### Step 5: SSH into the temporary instance  
Mount the volume:
```bash
sudo mkdir /mnt/recovery
sudo mount /dev/xvdf1 /mnt/recovery
```

Edit the `authorized_keys` file on the broken instance's volume:
```bash
sudo nano /mnt/recovery/home/ec2-user/.ssh/authorized_keys
```

Add the **public key** from your new key pair (`new-key.pub`)

---

#### Step 6: Detach the volume from the rescue instance  
- Unmount the volume  
- Detach it and re-attach to the original instance as `/dev/xvda`

---

#### Step 7: Start the original instance and SSH with new key  
```bash
ssh -i new-key.pem ec2-user@<public-ip>
```

---

### 🧠 Prevention Tips:

- Always back up PEM files in a secure location (like a password manager).
- Create a **secondary user** with a different key for backup access.
- Use EC2 Instance Connect for temporary browser-based access (only works for Amazon Linux 2+ and enabled roles).

> Summary:  
> You cannot restore a PEM file, but you can regain access by editing the `authorized_keys` on the root volume through a temporary rescue EC2 instance.

---
