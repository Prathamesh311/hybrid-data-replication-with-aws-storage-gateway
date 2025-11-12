# ☁️ Hybrid Data Replication using AWS Storage Gateway

This project demonstrates how to integrate on-premises storage with AWS S3 using **AWS Storage Gateway (File Gateway)** for seamless **hybrid data replication**.  
It automatically syncs files created in a local NFS mount to an Amazon S3 bucket.

---

## 🚀 Project Overview

The **Hybrid Data Replication using AWS Storage Gateway** project demonstrates how to connect on-premises infrastructure with **AWS Cloud storage** to achieve seamless, secure, and automatic data synchronization.

In this project, we use the **AWS Storage Gateway (File Gateway)** service to bridge on-premises storage (simulated using an EC2 instance) and **Amazon S3**.  
Any file created or modified in the local NFS mount directory is automatically replicated to the connected **S3 bucket**, ensuring **data durability, accessibility, and backup consistency**.

This hybrid architecture allows organizations to:
- Maintain **local access speed** for frequently used data (via gateway caching)
- Benefit from **AWS scalability and redundancy**
- Simplify **backup, restore, and archival** operations
- Minimize the need for complex manual synchronization or data transfer jobs

### 🔑 Key Features
- **Hybrid Cloud Integration:** Seamlessly connects on-premises workloads to AWS S3.  
- **Automatic Data Replication:** Any local file changes are synced to the cloud in near real-time.  
- **File Gateway Protocols:** Supports **NFS** or **SMB** for compatibility with existing systems.  
- **Secure and Durable:** Data stored in **S3** benefits from 99.999999999% (11 nines) durability.  
- **Scalable Architecture:** Easily extend storage capacity without upgrading local hardware.  
- **Use Cases:** Backup, disaster recovery, hybrid workloads, and data migration.

---

## 🛠️ Step-by-Step Implementation

### **1️⃣ Create Storage Gateway**
- Open the **AWS Storage Gateway Console**
- Choose **File Gateway** as the gateway type
- Deploy gateway on **Amazon EC2**

📸 *Screenshot:*  
![Storage Gateway](SS/StorageGateway.png)

---

### **2️⃣ Launch Gateway Appliance**
- Launch an EC2 instance using the **AWS Storage Gateway AMI**
- Recommended instance type: **m5.xlarge** (with 80GB root and 150GB additional volume)
- Ensure **All Traffic** is temporarily allowed in the default Security Group (for setup)
- Note down the **public IP** of the instance — this will be used to activate the gateway

📸 *Screenshot:*  
![Gateway Appliance](SS/Gateway-Applience.png)

---

### **3️⃣ Activate the Gateway**
- Go to the **Storage Gateway Console** and click **“Activate Gateway”**
- Enter the public IP of your EC2 appliance
- Complete the activation steps

📸 *Screenshot:*  
![Activate Gateway](SS/Avtivate-Gateway.png)

---

### **4️⃣ Create an S3 Bucket**
- Go to **Amazon S3** and create a new private bucket 
- Keep the bucket **empty** for now

📸 *Screenshot:*  
![Empty Bucket](SS/Empty-bkt.png)

---

### **5️⃣ Configure File Share**
- In the **Storage Gateway Console**, choose **Create File Share**
- Select **Amazon S3 bucket** as the target
- Choose **NFS** as the access protocol
- Configure other settings and create the file share

📸 *Screenshot:*  
![File Share Protocol](SS/fileshare-protocol.png)

---

### **6️⃣ File Share Created Successfully**
- Once created, note the **Mount Command** displayed for your NFS client
- This command will be used to mount your file share on EC2

📸 *Screenshot:*  
![File Share Created](SS/created-fileshare.png)

---

### **7️⃣ Mount File Share on EC2**
- Connect to your EC2 instance via SSH and run the following commands:
```bash
sudo -s
yum install -y nfs-utils
mkdir filesystem
sudo mount -t nfs -o nolock,hard 172.31.47.158:/private-backup-bucket-1 filesystem/
cd filesystem
ls
touch hello.txt
touch welcome.txt
```
![mounted](SS/mounted.png)

### **8.  Successfully replicated backup on S3 bucket**
*Screenshot:*
![UpdatedBucket](SS/updated-bkt.png)

---

By implementing this project, you will understand:
- How to deploy and activate a **File Gateway**  
- How to configure **file shares** linked to **S3 buckets**  
- How local NFS file operations are automatically synchronized to AWS  
- The inner working of hybrid cloud replication and caching mechanisms.


