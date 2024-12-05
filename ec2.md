# AWS EC2 Cheatsheet

## EC2 IP Addressing
### Public and Private IPs
- **Public IP Address**
  - Automatically assigned to an instance in a public subnet.
  - Released when the instance stops.
  - Accessible over the internet.

- **Elastic IP (EIP)**
  - Static IPv4 address you can associate with an instance.
  - Detachable and reassignable.
  - Not charged when attached to a running instance.

### Key Concepts
- Instances launched in a **VPC** have private IPs by default.
- To use a public IP, enable the option during instance launch or associate an Elastic IP.

---

## Placement Groups
### Types of Placement Groups
1. **Cluster Placement Group**
   - Instances placed close together in a single Availability Zone (AZ).
   - Provides low latency and high network throughput.
   - Use case: HPC (High-Performance Computing) or tightly-coupled workloads.
   - **Key Considerations:**
     - All instances must be launched within the same AZ.
     - May face capacity issues; plan ahead for instance launches.

2. **Spread Placement Group**
   - Instances placed on distinct hardware across different AZs.
   - Limits: Max 7 running instances per AZ.
   - Use case: High availability and fault tolerance.
   - **Key Considerations:**
     - Suitable for applications that require critical isolation between instances.
     - Each instance runs on separate physical hardware, minimizing risk.

3. **Partition Placement Group**
   - Divides instances into logical partitions, each on separate hardware.
   - Use case: Big data and distributed systems.
   - **Key Considerations:**
     - Up to 7 partitions per AZ are supported.
     - Each partition can hold multiple instances, ensuring fault isolation.
     - AWS provides information on which instance belongs to which partition.

### Best Practices
- Choose the placement group type based on application requirements.
- Use enhanced networking for better performance (e.g., Elastic Fabric Adapter).

---

## Networking
### Security Groups
- Stateful virtual firewall controlling inbound and outbound traffic.
- Rules apply immediately when modified.
- Allow only necessary ports and IP ranges.

### Network Access Control Lists (NACLs)
- Stateless subnet-level firewall.
- Supports allow and deny rules.
- Rules evaluated in numerical order.

### Elastic Load Balancer (ELB)
- Distributes traffic across instances.
- Types:
  - Application Load Balancer (ALB): HTTP/HTTPS.
  - Network Load Balancer (NLB): High performance for TCP/UDP.

---

## Monitoring and Scaling
### Amazon CloudWatch
- Collects and monitors metrics, logs, and events.
- Create alarms for specific metrics (e.g., CPU utilization).

### Auto Scaling
- Automatically adjusts the number of instances based on demand.
- Policies:
  - Target tracking: Maintain a specific metric level.
  - Step scaling: Based on step adjustments.

---

## Pricing and Optimization
### EC2 Pricing Models
1. **On-Demand Instances**
   - No upfront costs, pay per second.
   - Use case: Short-term or unpredictable workloads.

2. **Spot Instances**
   - Use unused EC2 capacity at a reduced price.
   - Risk of termination if capacity is needed.
   - Use case: Flexible and fault-tolerant workloads.

### Cost Optimization Tips
- Right-size instances based on workload.
- Use Spot Instances for non-critical tasks.
- Monitor with AWS Cost Explorer.

---

## Key AWS CLI Commands
- List instances:
  ```bash
  aws ec2 describe-instances
  ```

- Start an instance:
  ```bash
  aws ec2 start-instances --instance-ids <instance_id>
  ```

- Stop an instance:
  ```bash
  aws ec2 stop-instances --instance-ids <instance_id>
  ```

- Allocate an Elastic IP:
  ```bash
  aws ec2 allocate-address
  ```

- Create a placement group:
  ```bash
  aws ec2 create-placement-group --group-name <name> --strategy <type>
  ```



# AWS Exam Cheatsheet: Elastic Network Interfaces (ENIs)

## What is an ENI?
- An **Elastic Network Interface (ENI)** is a virtual network card attached to an EC2 instance.
- It allows instances to communicate within the VPC and to the internet (if configured).

---

## What is a VPC?
- A **Virtual Private Cloud (VPC)** is a virtual network dedicated to your AWS account.
- It is logically isolated from other virtual networks in AWS.

### Key Components of a VPC:
1. **Subnets**:
   - Subsections of a VPC’s IP address range.
   - Can be public (accessible from the internet) or private (no direct internet access).

2. **Route Tables**:
   - Define how traffic is directed within the VPC and beyond.
   - Can route traffic to the internet via an Internet Gateway.

3. **Internet Gateway (IGW)**:
   - Allows communication between resources in your VPC and the internet.

4. **NAT Gateway**:
   - Enables private instances to access the internet without exposing them to inbound traffic.

5. **Security Groups and NACLs**:
   - **Security Groups:** Instance-level firewalls that control inbound and outbound traffic.
   - **NACLs (Network ACLs):** Subnet-level firewalls that control traffic.

6. **CIDR Block**:
   - A range of IP addresses assigned to your VPC (e.g., 10.0.0.0/16).

### Why is a VPC Important for ENIs?
- ENIs operate within the scope of a VPC.
- Subnets in a VPC define the IP addressing for ENIs.
- VPC configurations (e.g., route tables, gateways) determine how ENIs communicate within and outside AWS.

---

## Key Features of ENIs
1. **Primary ENI**:
   - Automatically created when you launch an EC2 instance.
   - Cannot be detached or deleted while the instance is running.

2. **Secondary ENIs**:
   - Additional ENIs can be attached to an instance.
   - Can be detached and attached to other instances.
   - Useful for network appliances or applications requiring multiple IP addresses.

3. **IP Addressing**:
   - Each ENI has a private IPv4 address from the subnet's CIDR block.
   - Can have multiple secondary private IP addresses.
   - Can optionally have an Elastic IP (EIP) for internet access.

4. **Security Groups**:
   - Each ENI can have its own security group.
   - Allows fine-grained control over traffic for multi-interface instances.

---

## EC2 Hibernate
- **What is Hibernate?**
   - Hibernate allows you to save the state of an EC2 instance and resume it later.
   - The operating system, applications, and in-memory data are preserved by saving them to the root volume (EBS).

- **How it Works:**
   - When you hibernate an instance, the instance state is "stopped," but the data in memory is stored on the root EBS volume.
   - When you start the instance again, the operating system and applications are restored to their previous state.

- **Key Requirements:**
   - The root volume must be an encrypted EBS volume.
   - Instance must use an AMI that supports hibernation (Amazon Linux 2, Ubuntu, Windows).
   - Compatible instance types include T2, T3, M3, M4, M5, C3, C4, C5, and R3 families.

- **Limitations:**
   - Hibernation is supported only for instances running for less than 60 days.
   - Instance storage (non-EBS) is not preserved.

- **Use Cases:**
   - Preserving session state for development or testing environments.
   - Quickly resuming long-running workloads without re-initializing applications.

---

## Use Cases
- **High Availability:** Attach a secondary ENI to a backup instance for quick failover.
- **Multiple IP Addresses:** Support for multi-IP applications (e.g., hosting multiple websites).
- **Separation of Traffic:** Use separate ENIs for public and private traffic.
- **Virtual Appliances:** Configure ENIs for firewalls, NAT gateways, or load balancers.

---

## ENI Operations (AWS CLI)
### Create an ENI:
```bash
aws ec2 create-network-interface --subnet-id <subnet_id> --description "My ENI" --groups <security_group_id>
```

### Attach an ENI to an Instance:
```bash
aws ec2 attach-network-interface --network-interface-id <eni_id> --instance-id <instance_id> --device-index 1
```

### Detach an ENI:
```bash
aws ec2 detach-network-interface --attachment-id <attachment_id>
```

### Delete an ENI:
```bash
aws ec2 delete-network-interface --network-interface-id <eni_id>
```

---

## Exam Tips
- Primary ENIs cannot be detached; secondary ENIs can.
- Secondary ENIs are useful for applications requiring failover or multiple IPs.
- Remember the role of security groups in ENI traffic control.
- Know how to create, attach, detach, and delete ENIs using the CLI.
- Understand how ENIs work with Elastic IPs for public internet access.
- Understand the relationship between ENIs, subnets, and route tables in a VPC.
- For Hibernate, remember the EBS encryption requirement and supported instance families.

---
---- 


# **AWS Exam Cheatsheet: EBS, AMI, and EFS**


### **EBS (Elastic Block Store)**
1. **Key Features**:
   - Persistent, block-level storage for EC2.
   - Automatically replicated within the same AZ.
   - Snapshots for backups, stored in S3.
   - Types:
     - **General Purpose SSD (gp3/gp2):** Balanced price/performance.
     - **Provisioned IOPS SSD (io1/io2):** High performance, low latency.
     - **Throughput Optimized HDD (st1):** Big data, high throughput.
     - **Cold HDD (sc1):** Low-cost infrequent access.

2. **Important Facts**:
   - Attached to one EC2 instance at a time.
   - Can resize or change volume types without downtime.
   - Cross-AZ use requires snapshots and recreating the volume.

---

### **AMI (Amazon Machine Image)**
1. **Key Features**:
   - Blueprint for launching EC2 instances.
   - Includes:
     - OS configuration (e.g., Linux, Windows).
     - Installed software/packages.
     - Permissions and storage volumes.

2. **Types**:
   - **Public AMI:** Shared by AWS or other users.
   - **Custom AMI:** User-created for specific configurations.
   - **AWS Marketplace AMI:** Paid/pre-configured AMIs.

3. **Important Facts**:
   - AMIs are regional (copy to use in other regions).
   - Can create AMIs from an EC2 instance.
   - Includes references to EBS or Instance Store.

---

### **EFS (Elastic File System)**
1. **Key Features**:
   - Managed, scalable file storage for Linux-based workloads.
   - Shared access for multiple EC2 instances.
   - Automatically grows/shrinks as files are added/removed.
   - **Storage Classes**:
     - Standard (frequently accessed).
     - Infrequent Access (lower cost for less frequent access).

2. **Use Cases**:
   - Data sharing between instances.
   - Content management, big data, and analytics.

3. **Important Facts**:
   - Accessible across multiple AZs in a region.
   - Supports NFSv4 protocol.
   - Charged based on storage and throughput.

---

### **Comparison Quick Reference**

| **Feature**       | **EBS**                          | **AMI**               | **EFS**                           |
|--------------------|----------------------------------|-----------------------|-----------------------------------|
| **Type**          | Block storage                   | Instance blueprint    | File storage                     |
| **Use Case**      | Attach to one EC2 instance       | Launch EC2 instances  | Shared file storage              |
| **Persistence**   | Persistent (one AZ)             | Not applicable        | Persistent (regional)            |
| **Scalability**   | Pre-defined size, resizable      | Not applicable        | Automatically scalable           |

**Tip:** Know EBS volume types, AMI lifecycle, and EFS use cases for the exam!

### **EBS Volume Types**:
1. **General Purpose SSD (gp3, gp2)**: Cost-effective, good for most workloads.
2. **Provisioned IOPS SSD (io1, io2)**: High-performance for I/O-intensive workloads.
3. **Throughput Optimized HDD (st1)**: High throughput for big data and streaming.
4. **Cold HDD (sc1)**: Low-cost for infrequent access.

### **EBS Volume Types Comparison Chart**

| **Volume Type**              | **Use Case**                                 | **IOPS**                                    | **Throughput**                     | **Cost**         | **Boot Volume Support** | **Key Features** |
|-------------------------------|----------------------------------------------|---------------------------------------------|-------------------------------------|------------------|--------------------------|-------------------|
| **gp3 (General Purpose SSD)**| Balanced performance, cost-efficient         | Baseline 3,000, configurable up to 16,000   | Baseline 125 MiB/s, up to 1,000 MiB/s | Moderate         | Yes                      | Independently provision IOPS and throughput |
| **gp2 (General Purpose SSD)**| General workloads with moderate performance  | 3 IOPS/GB (burst up to 3,000 IOPS for <1 TiB), max 16,000 | Up to 250 MiB/s                   | Moderate         | Yes                      | Performance scales with volume size |
| **io1 (Provisioned IOPS SSD)**| I/O-intensive workloads with low latency     | Provisioned up to 64,000                   | Up to 1,000 MiB/s                   | High             | Yes                      | Predictable, high IOPS for databases |
| **io2 (Provisioned IOPS SSD)**| High durability and I/O-intensive workloads  | Provisioned up to 64,000 (256,000 with io2 Block Express) | Up to 1,000 MiB/s (4,000 MiB/s with Block Express) | High             | Yes                      | 99.999% durability, great for critical apps |
| **st1 (Throughput Optimized HDD)**| Big data, log processing, data warehouses | Up to ~500 IOPS                            | Up to 500 MiB/s                     | Low              | No                       | High sequential throughput |
| **sc1 (Cold HDD)**            | Infrequently accessed data                  | Up to ~250 IOPS                            | Up to 250 MiB/s                     | Very Low         | No                       | Lowest cost per GB for cold storage |
---

### **Comparison: io1 vs io2 Block Express vs Instance Store**

| Feature                  | **io1**                            | **io2 Block Express**                   | **Instance Store**                  |
|--------------------------|-------------------------------------|-----------------------------------------|-------------------------------------|
| **Max IOPS per Volume**  | 64,000                             | 256,000                                | Millions                           |
| **Max Throughput**       | 1,000 MiB/s                        | 4,000 MiB/s                            | Extremely high (direct hardware access) |
| **Durability**           | 99.9%                              | 99.999%                                | Ephemeral (data lost on stop/terminate) |
| **Performance Scaling**  | Limited                            | Scales linearly with multiple volumes. | Direct, no scaling needed          |
| **Cost Efficiency**      | Higher cost per IOPS/durability    | Better IOPS and durability for the cost. | Cost included in instance pricing |
| **Use Case**             | I/O-intensive workloads with high durability | Critical workloads needing high durability and scalability | Ultra-high-performance, ephemeral workloads |


### **Key Notes**:
- **Performance Dependency:** Max IOPS and throughput often require **Nitro-based EC2 instances**.
- **Boot Volume Support:** Only **gp2**, **gp3**, **io1**, and **io2** can be used as boot volumes.
- **Durability:** **io2** provides the highest durability at **99.999%**.
- **Scaling Differences:** 
  - **gp2**: IOPS increases with volume size.
  - **gp3**: IOPS and throughput are provisioned independently of size.
- **Cost Optimization:** Use **gp3** for most workloads, **io2** for critical I/O workloads, and **HDD volumes** (st1/sc1) for cost-sensitive archival or streaming needs.

**Note**: HDD volumes (**st1**, **sc1**) **cannot** be used as boot volumes because they do not support the required low-latency random I/O operations.

## **EBS Multi-Attach:** ### 
Allows a single **io1** or **io2** EBS volume to be attached to multiple EC2 instances simultaneously within the same Availability Zone. 

### Key Points:
- **Use Case**: Enables shared data access for clustered or distributed applications.
- **Limitations**: 
  - Supported only for **io1** and **io2** volumes.
  - Applications must handle concurrent write/reads to avoid data corruption.
- **Maximum Attachments**: Up to 16 EC2 instances per volume.

## Steps to encrypt an **unencrypted EBS volume** attached to your EC2 instance:

1. **Create a Snapshot**:
   - Take a snapshot of the unencrypted volume (this snapshot will also be unencrypted).

2. **Copy the Snapshot with Encryption**:
   - Copy the unencrypted snapshot and enable encryption during the copy process.
   - Specify a KMS key for encryption (default or custom).

3. **Create an Encrypted Volume**:
   - Create a new EBS volume from the encrypted snapshot.

4. **Attach the Encrypted Volume**:
   - Detach the original unencrypted volume from the EC2 instance.
   - Attach the new encrypted volume to the instance.

5. **Update Mount Points**:
   - Ensure the new volume is mounted to the same location as the original volume in the instance.

This method ensures no downtime and secures your data on the volume.