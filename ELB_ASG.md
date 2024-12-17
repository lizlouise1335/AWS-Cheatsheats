Here are concise exam notes tailored for **AWS Associate-level** on **Elastic Load Balancing (ELB)** and **Auto Scaling Groups (ASG)**. 

---

# **Elastic Load Balancing (ELB)**

### **Overview**  
- ELB **distributes incoming traffic** across multiple targets (EC2, containers, Lambda functions).  
- Supports fault tolerance, scalability, and high availability.  
- **Types of ELBs**:  
  - **Application (ALB)**  
  - **Network (NLB)**  
  - **Gateway (GWLB)**  
  - **Classic (CLB)**  

---

### **Application Load Balancer (ALB)**  
- Operates at **Layer 7 (Application Layer)**.  
- **Use Cases**: HTTP/HTTPS traffic, advanced routing.  
- **Features**:  
  - Path-based routing (`/images` → Target Group 1).  
  - Host-based routing (e.g., `example.com`).  
  - Supports **WebSockets** and **HTTP/2**.  
  - **Target Groups**: EC2, Lambda, IP addresses.  
  - Health checks at the target level.  
- **Listener Rules**: Forward, redirect, fixed response actions.  

---

### **Network Load Balancer (NLB)**  
- Operates at **Layer 4 (Transport Layer)**.  
- **Use Cases**: TCP/UDP traffic, high-performance and low-latency applications.  
- **Features**:  
  - **Static IP addresses** for the load balancer.  
  - Targets include EC2 instances, IP addresses.  
  - Handles millions of requests per second.  
  - **Preserves client IP** for targets.  

---

### **Gateway Load Balancer (GWLB)**  
- Operates at **Layer 3**.  
- **Use Case**: Deploy virtual appliances (e.g., firewalls).  
- Combines:  
  - Load balancing  
  - Transparent network gateway  

---

### **Classic Load Balancer (CLB)**  
- **Legacy ELB**, operates at **Layer 4 (TCP/SSL)** and **Layer 7 (HTTP/HTTPS)**.  
- Limited feature set.  
- Health checks for EC2 instances.  
- **Deprecated** for new applications.  

---

### **Cross-Zone Load Balancing**  
- Ensures traffic is evenly distributed across instances in all **AZs**.  
- **ALB**: Enabled by default.  
- **NLB**: Disabled by default but configurable.

---

### **Sticky Sessions**  
- Ensures a client is consistently routed to the same target.  
- Works for **HTTP/HTTPS** (ALB) or **TCP/SSL** (CLB).  

---

### **SSL/TLS Certificates**  
- Managed using **AWS Certificate Manager (ACM)**.  
- Can terminate SSL on ELB.

---

### **ELB Monitoring**  
- **Metrics**: Requests count, latency, unhealthy/healthy host count.  
- Integrated with **CloudWatch** for alarms.  
- **Access Logs**: Saved in **S3**.  

---

# **Auto Scaling Groups (ASG)**

### **Overview**  
- **Automatically adjusts EC2 instance capacity** based on demand.  
- Ensures high availability and cost-efficiency.  

---

### **Key Components**  
1. **Launch Configuration/Launch Template**  
   - Defines instance type, AMI, security group, and key pair.  
   - Launch Template is **recommended** over Launch Configuration.  
2. **Auto Scaling Group**  
   - Logical group of EC2 instances.  
   - Associated with **minimum, maximum, and desired capacity**.  
3. **Scaling Policies**  
   - **Dynamic Scaling**: Based on metrics.  
     - **Target Tracking**: Maintain a target value (e.g., CPU at 50%).  
     - **Step Scaling**: Scale in steps based on thresholds.  
   - **Scheduled Scaling**: Increase/decrease capacity at specific times.  
4. **Health Checks**  
   - Monitor instances using **EC2 status checks** or **ELB health checks**.  
   - Replace unhealthy instances automatically.  

---

### **Auto Scaling Lifecycle**  
1. **Pending**: Instance is being launched.  
2. **InService**: Running and receiving traffic.  
3. **Terminating**: Instance is being terminated.  
4. **Standby**: Temporarily removed from scaling activities.

---

### **Termination Policies**  
- **Default**: Closest to AZ balance and oldest launch configuration.  
- Options:  
  - **Oldest Instance**  
  - **Newest Instance**  
  - **AZ Rebalancing**  

---

### **Monitoring ASG**  
- **CloudWatch Metrics**: CPU utilization, network traffic, etc.  
- **Alarms** trigger scaling policies.

---

### **ELB & ASG Integration**  
- **ELB** distributes traffic to instances in the ASG.  
- **ASG** scales instances **automatically** based on policies.  
- Ensure proper health checks between ELB and ASG.  

---

### **Key Notes**  
- Always use **Launch Templates** for flexibility.  
- ELB + ASG = High Availability & Fault Tolerance.  
- Understand how **Target Tracking** and **Step Scaling** work.  
- Monitor ASG lifecycle transitions in **CloudWatch**.

---

These notes summarize the main points for AWS **ELB** and **ASG** in preparation for the **AWS Associate Certification Exam**. Let me know if you'd like additional details or diagrams!