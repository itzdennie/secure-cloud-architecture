# Secure Cloud Architecture Plan

## Architecture Overview
Users → CDN → Load Balancer → Application Servers → Private Database

## Component Descriptions

### CDN
The CDN stores cached copies of static content closer to users to improve loading speed.

### Load Balancer
The load balancer distributes incoming requests across multiple application servers.

### Application Servers
Application servers process requests from users. These servers should be placed in a private subnet.

### Database
The database stores student records. The database should remain private and should not be directly accessible from the Internet.

---

# Public and Private Resources

| Resource | Public or Private? | Explanation |
| :--- | :--- | :--- |
| **CDN** | Public | Stores cached static assets closer to end users for faster access. |
| **Load Balancer** | Public | Direct entry point from the Internet that routes user requests to backend servers. |
| **Application Server** | Private | Processes application logic and should remain isolated from direct Internet access. |
| **Database** | Private | Contains student records and must strictly block direct Internet access. |

---

# Security Controls

### IAM
Access should be strictly granted using the Principle of Least Privilege so users only get permissions necessary for their role.

### MFA
Multi-Factor Authentication should be mandatory for all administrative and user accounts to add an extra layer of protection beyond passwords.

### Firewall / Security Group
Allowed connections:
* Internet → Load Balancer = Allowed
* Load Balancer → Application Server = Allowed
* Application Server → Database = Allowed
* Internet → Database = Blocked
* Internet → Application Server = Blocked

### Encryption
Student information must be encrypted at rest and in transit to protect sensitive data from theft or interception.

### Logging
All user authentication attempts, system events, and network requests should be recorded for security auditing.

### Monitoring
System traffic and resource usage should be monitored for unusual spikes, unauthorized login attempts, and suspicious activities.

### Backup
Regular database backups ensure data can be recovered in case of accidental deletion, hardware failure, or ransomware attacks.

---

# Principle of Least Privilege

| User | Allowed Access |
| :--- | :--- |
| **Administrator** | Full system access to configure cloud infrastructure and user permissions. |
| **Instructor** | Access to view and update student records and class information. |
| **Student** | Read-only access to view their personal student records. |
| **Developer** | Access to modify application code and testing environments, but no access to production database records. |

---

# Shared Responsibility Model

| Responsibility | Cloud Provider or Customer? |
| :--- | :--- |
| Physical data center | Cloud Provider |
| Physical servers | Cloud Provider |
| User accounts | Customer |
| Student data | Customer |
| IAM permissions | Customer |
| Application security | Customer |
| Database access rules | Customer |
| Backups | Customer |

### Security OF the Cloud
This refers to the security of the underlying infrastructure, physical hardware, data centers, and network facilities managed by the cloud provider.

### Security IN the Cloud
This refers to the security configurations, user permissions, application code, data encryption, and access controls managed by the customer.

---

# Architecture Questions

1. **Which resource should be directly accessible from the Internet?**
   The CDN and the Load Balancer.

2. **Why should the database remain private?**
   To protect sensitive student records from direct exposure and external attacks.

3. **Why should users not connect directly to the database?**
   To prevent unauthorized queries, data tampering, and bypassing of application security logic.

4. **What is the purpose of a load balancer?**
   To distribute incoming web traffic across multiple application servers to prevent overloading.

5. **What happens if one application server fails?**
   The load balancer redirects incoming requests to the remaining operational application servers.

6. **What is the purpose of a CDN?**
   To cache static assets geographically closer to users to improve site loading speed.

7. **Why should administrator accounts use MFA?**
   Because they have elevated privileges, and MFA adds an extra defense layer against password compromise.

8. **Why should administrator access not be given to every employee?**
   To enforce the principle of least privilege and minimize security risks or accidental misconfigurations.

9. **Why are logging and monitoring important?**
   They help track system activity, detect suspicious behavior, and audit security events.

10. **Why are backups important?**
    They allow data restoration in case of system failures, corruption, or cyberattacks.
