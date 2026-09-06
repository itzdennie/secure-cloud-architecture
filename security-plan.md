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
Explain who should have access to the cloud environment:
Access should be strictly granted using the Principle of Least Privilege so users only get permissions necessary for their role.

### MFA
Explain which accounts should use Multi-Factor Authentication:
Multi-Factor Authentication should be mandatory for all administrative and user accounts to add an extra layer of protection beyond passwords.

### Firewall / Security Group
Explain what connections should be allowed:
* Internet → Load Balancer = Allowed
* Load Balancer → Application Server = Allowed
* Application Server → Database = Allowed
* Internet → Database = Blocked
* Internet → Application Server = Blocked

### Encryption
Explain why student information should be encrypted:
Student information must be encrypted at rest and in transit to protect sensitive data from theft or interception.

### Logging
Explain what activities should be recorded:
All user authentication attempts, system events, and network requests should be recorded for security auditing.

### Monitoring
Explain what suspicious activity should be monitored:
System traffic and resource usage should be monitored for unusual spikes, unauthorized login attempts, and suspicious activities.

### Backup
Explain why the database should have backups:
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

---

# Architecture Questions

### 1. What does Security OF the Cloud mean?
Security OF the Cloud refers to the security of the underlying physical infrastructure, data centers, server hardware, and core networking facilities managed directly by the cloud provider.

### 2. What does Security IN the Cloud mean?
Security IN the Cloud refers to the security configurations, user account access controls, application logic, database security rules, firewall policies, and data encryption managed by the customer.

### 3. Which resource should be directly accessible from the Internet?
The CDN and the Load Balancer.

### 4. Why should the database remain private?
To protect sensitive student records from direct public exposure and unauthorized access from external network threats.

### 5. Why should users not connect directly to the database?
To prevent direct database queries that bypass application authentication, input validation, and business logic, which could lead to data tampering or leaks.

### 6. What is the purpose of a load balancer?
To distribute incoming user web traffic evenly across multiple application servers to optimize network efficiency and prevent system overload.

### 7. What happens if one application server fails?
The load balancer automatically detects the failure and reroutes user traffic to the remaining healthy application servers without interrupting service.

### 8. What is the purpose of a CDN?
To cache static web content geographically closer to users across edge locations to significantly increase loading speeds.

### 9. Why should administrator accounts use MFA?
Because administrative accounts hold elevated access privileges; requiring MFA prevents total account takeover even if passwords are stolen.

### 10. Why should administrator access not be given to every employee?
To enforce the Principle of Least Privilege and minimize administrative errors, security risks, or unauthorized access modifications.

### 11. Why are logging and monitoring important?
They provide complete visibility into system activities, allowing security teams to track actions, audit events, and detect suspicious behavior in real time.

### 12. Why are backups important?
They ensure critical data can be quickly recovered in the event of system failures, database corruption, accidental deletion, or ransomware incidents.
