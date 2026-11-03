# Enterprise AWS Cloud Infrastructure and Multi-Tier Application Deployment

## Project Overview

This project focuses on designing, building, and documenting a secure, highly available cloud infrastructure on Amazon Web Services (AWS).

The goal is to simulate a real-world enterprise environment by implementing AWS networking, compute, load balancing, automatic scaling, storage, database services, monitoring, and security.

The infrastructure is being developed and validated step by step, with practical implementation evidence and troubleshooting documentation.

**Project Status:** In Progress

## Project Objectives

- Design a custom AWS Virtual Private Cloud (VPC).
- Build a network across two Availability Zones.
- Separate public-facing and private application resources.
- Configure secure connectivity using Internet Gateway, NAT Gateway, route tables, and security groups.
- Deploy EC2 application servers in private subnets.
- Configure a Bastion Host for administrative access.
- Deploy an Application Load Balancer (ALB).
- Configure Auto Scaling for application instances.
- Integrate managed database and storage services.
- Implement infrastructure monitoring and auditing.
- Document implementation steps, testing, and troubleshooting.

## AWS Services and Technologies

| Category | AWS Services / Technologies |
|---|---|
| Networking | VPC, Subnets, Internet Gateway, NAT Gateway, Route Tables |
| Compute | Amazon EC2 |
| Load Balancing | Application Load Balancer |
| Scaling | Auto Scaling Group, Launch Templates |
| Storage | EBS, EFS, S3 |
| Database | Amazon RDS (MySQL) |
| Monitoring | Amazon CloudWatch |
| Auditing | AWS CloudTrail |
| Operating System | Amazon Linux |
| Web Server | Apache HTTP Server |
| Version Control | Git and GitHub |

## Architecture Overview

The project uses a multi-tier architecture distributed across two AWS Availability Zones.

### Planned Architecture Components

- **Public Subnets:** Host the Application Load Balancer and Bastion Host.
- **Private Subnets:** Host application EC2 instances and database resources.
- **Internet Gateway:** Provides internet connectivity to eligible public subnet resources.
- **NAT Gateway:** Allows private instances to initiate outbound internet connections.
- **Application Load Balancer:** Distributes incoming application traffic across healthy targets.
- **Auto Scaling Group:** Maintains application capacity based on configured scaling policies.
- **Amazon RDS:** Provides managed MySQL database functionality.
- **Amazon EFS:** Provides shared file storage.
- **Amazon S3:** Stores application files and backups.
- **CloudWatch and CloudTrail:** Support monitoring, alarms, and account activity auditing.

The architecture diagram and final traffic flow will be added after the infrastructure components have been implemented and verified.

## Implementation Progress

| Component | Current Status |
|---|---|
| Custom VPC | Completed |
| Two Public Subnets | Completed |
| Two Private Subnets | Completed |
| Internet Gateway | Completed |
| Public Route Table | Completed |
| Private Route Table | Completed |
| NAT Gateway | Completed |
| Bastion Host | Completed |
| Private EC2 Instances | Completed |
| SSH Agent Forwarding | Verified |
| Apache Web Server | Installed and verified on both application servers |
| Application Load Balancer | Active and tested |
| Target Group Health Checks | Both application targets verified healthy |
| Launch Template Version 2 | Created |
| Auto Scaling Group Update | Pending |
| RDS MySQL | Planned |
| EBS | Planned |
| EFS | Planned |
| S3 | Planned |
| CloudWatch | Planned |
| CloudTrail | Planned |

*Statuses reflect implementation progress at the time this README was prepared. They will be updated as additional testing and implementation are completed.*

## Network Design

The project uses a custom VPC with separate public and private subnets across two Availability Zones.

### Public Network

- Hosts public-facing infrastructure.
- Contains the Bastion Host.
- Provides the network placement for the Application Load Balancer.
- Uses an Internet Gateway for internet connectivity.

### Private Network

- Hosts application EC2 instances.
- Keeps application servers away from direct public internet exposure.
- Uses NAT Gateway routing for required outbound internet access.
- Will host private database resources.

## Security Implementation

Security is an important part of this project.

The following security practices are being implemented:

- Separate public and private network tiers.
- Security Group-based traffic restrictions.
- Restricted SSH access to the Bastion Host.
- Private application server access through the Bastion Host.
- No direct public IP requirement for private application instances.
- Least-privilege access as the project progresses.
- Secure handling of credentials and configuration information.

## Testing and Validation

The following tests have been performed:

- Verified VPC and subnet creation.
- Verified Internet Gateway and NAT Gateway configuration.
- Verified public and private route table associations.
- Verified Bastion Host SSH connectivity.
- Verified SSH access to both private application servers.
- Verified Apache installation and service status on both application servers.
- Verified local HTTP responses from both application servers.
- Verified Application Load Balancer accessibility.
- Verified both registered application targets are healthy.
- Verified that requests through the load balancer can reach both application servers.

Additional testing will be documented as the project progresses.

## Troubleshooting Scenarios

The project includes practical troubleshooting exercises covering:

1. EC2 instance connectivity failures.
2. Application Load Balancer unhealthy targets.
3. Private EC2 internet connectivity problems.
4. Application-to-RDS database connectivity.
5. Auto Scaling instance launch failures.
6. EFS mounting issues.

Actual errors, root causes, solutions, and verification results will be recorded in the troubleshooting documentation.

## Repository Structure

The documentation and project evidence will be organized as follows:

```text
enterprise-aws-cloud-infrastructure/
│
├── README.md
│
├── architecture/
│   └── architecture-diagram.png
│
├── docs/
│   ├── networking/
│   ├── compute/
│   ├── load-balancing/
│   ├── auto-scaling/
│   ├── database/
│   ├── storage/
│   ├── monitoring/
│   └── security/
│
├── screenshots/
│   ├── vpc/
│   ├── ec2/
│   ├── alb/
│   ├── auto-scaling/
│   ├── database/
│   └── monitoring/
│
└── troubleshooting/
    └── troubleshooting-log.md
```

## Future Improvements

After completing the initial AWS infrastructure, the project can be extended with:

- Infrastructure as Code using Terraform.
- CI/CD pipeline using Jenkins.
- Docker containerization.
- Kubernetes deployment using Amazon EKS.
- GitOps deployment using ArgoCD.
- Advanced CloudWatch dashboards and alarms.
- Improved security and cost optimization.

These are future learning and development opportunities, not claimed as completed features of this project.

## Learning Outcomes

Through this hands-on project, the following practical skills are being developed:

- AWS cloud infrastructure design.
- VPC and network administration.
- Linux server administration.
- EC2 provisioning and management.
- SSH connectivity and secure access.
- Application Load Balancer configuration.
- Auto Scaling and high availability concepts.
- AWS monitoring and troubleshooting.
- Technical documentation.
- Git and GitHub version control.

## Disclaimer

This is a hands-on educational and portfolio project designed to demonstrate practical AWS cloud engineering skills. It is not a claim of a production deployment for a real organization.

All infrastructure components will be documented according to their actual implementation and verification status.

---

**Project Domain:** Cloud Computing / AWS Infrastructure

**Primary Platform:** Amazon Web Services (AWS)

**Repository:** Enterprise AWS Cloud Infrastructure and Multi-Tier Application Deployment
