### **AWS Vocab Sheet for EBS, AMI, and EFS**

---

#### **EBS (Elastic Block Store)**
1. **Block-Level Storage**: Storage where data is organized into fixed-sized blocks, allowing random read/write access.
2. **Persistent Storage**: Data remains intact even when the associated EC2 instance is stopped or terminated.
3. **AZ (Availability Zone)**: A distinct data center within a region with independent power and networking.
4. **Snapshots**: Point-in-time backups of EBS volumes stored in S3.
5. **General Purpose SSD (gp2/gp3)**: EBS volume type for balanced performance and cost.
6. **Provisioned IOPS SSD (io1/io2)**: High-performance EBS volume type for latency-sensitive workloads.
7. **Throughput Optimized HDD (st1)**: Cost-effective EBS volume type for large datasets needing high throughput.
8. **Cold HDD (sc1)**: Low-cost EBS volume type for infrequently accessed data.

---

#### **AMI (Amazon Machine Image)**
1. **Blueprint**: A predefined template for launching EC2 instances, including OS and software configurations.
2. **EC2 Instance**: A virtual server running in the AWS cloud.
3. **Public AMI**: An AMI shared publicly by AWS or other users.
4. **Custom AMI**: User-created AMI tailored for specific needs.
5. **AWS Marketplace AMI**: Paid or pre-configured AMIs for specific applications.
6. **Region**: A geographical location containing AWS data centers.
7. **EBS-backed AMI**: An AMI using EBS for instance storage.
8. **Instance Store-backed AMI**: An AMI using ephemeral instance storage.

---

#### **EFS (Elastic File System)**
1. **File Storage**: Storage where data is organized into hierarchical directories and files.
2. **Shared Access**: Multiple EC2 instances can access the same file system concurrently.
3. **NFSv4 (Network File System Version 4)**: A protocol for accessing files over a network.
4. **Standard Storage Class**: EFS storage tier for frequently accessed data.
5. **Infrequent Access (IA) Storage Class**: EFS storage tier for rarely accessed data, offering lower costs.
6. **Scalability**: Ability to grow or shrink storage automatically based on demand.
7. **Regional Service**: Accessible across all AZs in a specific AWS region.

---

#### **General AWS Terms**
1. **Durability**: Ability of a storage system to ensure data is not lost or corrupted over time.
2. **Latency**: The delay in data transmission or processing.
3. **Throughput**: The rate of data transfer, typically measured in MB/s.
4. **Ephemeral Storage**: Temporary storage that is deleted when the associated resource is terminated.
5. **Backup**: A copy of data stored for recovery in case of failure.

---

Use this sheet to quickly understand the core concepts and terminology for the AWS exam!