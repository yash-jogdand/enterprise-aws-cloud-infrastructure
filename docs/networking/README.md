
# AWS VPC and Networking Documentation

## 1. Overview

This document describes the VPC networking implementation for the Enterprise AWS Cloud Infrastructure project.

The infrastructure is deployed in the AWS Mumbai Region (`ap-south-1`) and uses a custom VPC with public and private subnets distributed across two Availability Zones.

## 2. VPC Configuration

| Property | Configuration |
|---|---|
| VPC Name | enterprise |
| AWS Region | ap-south-1 (Mumbai) |
| IPv4 CIDR | Record actual VPC CIDR |
| DNS Resolution | Verify in AWS Console |
| DNS Hostnames | Verify in AWS Console |

## 3. Subnet Design

The VPC contains four subnets.

| Subnet | Type | Availability Zone | CIDR |
|---|---|---|---|
| Public Subnet A | Public | Record actual AZ | Record actual CIDR |
| Public Subnet B | Public | Record actual AZ | Record actual CIDR |
| Private Subnet A | Private | Record actual AZ | Record actual CIDR |
| Private Subnet B | Private | Record actual AZ | Record actual CIDR |

### Public Subnets

Public subnets are used for internet-facing infrastructure.

Resources include:
- Bastion Host
- NAT Gateway
- Application Load Balancer

Public subnet route tables provide a default route to the Internet Gateway.

### Private Subnets

Private subnets host application EC2 instances.

The application servers do not require direct public internet exposure.

Their route table uses the NAT Gateway for outbound internet connectivity.

## 4. Internet Gateway

An Internet Gateway is attached to the enterprise VPC.

Purpose:
- Enables internet connectivity for eligible public subnet resources.
- Provides the internet route used by the public route table.
- Supports inbound and outbound internet traffic subject to routing and security rules.

## 5. NAT Gateway

A NAT Gateway has been deployed in Public Subnet A.

Purpose:
- Allows private subnet instances to initiate outbound internet connections.
- Prevents private instances from requiring public IPv4 addresses for normal outbound access.
- Supports package installation and software updates.

Current design limitation:

Only one NAT Gateway has been deployed. A production high-availability design may use a NAT Gateway in each Availability Zone to reduce cross-AZ dependency and improve resilience.

## 6. Route Table Design

### Public Route Table

Expected routes:

| Destination | Target |
|---|---|
| VPC CIDR | Local |
| 0.0.0.0/0 | Internet Gateway |

Associated subnets:
- Public Subnet A
- Public Subnet B

### Private Route Table

Expected routes:

| Destination | Target |
|---|---|
| VPC CIDR | Local |
| 0.0.0.0/0 | NAT Gateway |

Associated subnets:
- Private Subnet A
- Private Subnet B

## 7. Network Traffic Flow

### Public Internet Access

1. An internet user sends a request.
2. The request reaches the public-facing Application Load Balancer.
3. The ALB forwards traffic to a healthy private EC2 application server.
4. The application server processes the request.
5. The response returns through the ALB.

### Private EC2 Outbound Access

1. A private EC2 instance initiates an outbound connection.
2. Its private route table forwards the request to the NAT Gateway.
3. The NAT Gateway forwards the traffic through the Internet Gateway.
4. The response returns through the NAT Gateway to the private instance.

### Administrative SSH Access

1. The administrator connects to the Bastion Host.
2. The Bastion Host provides controlled access to private EC2 instances.
3. SSH agent forwarding is used for administrative connections to the application servers.

## 8. Security Considerations

- Keep application servers in private subnets.
- Restrict Bastion Host SSH access to the administrator's permitted IP address.
- Allow application HTTP traffic from the ALB security group.
- Avoid unnecessary inbound rules.
- Use route tables to control network paths.
- Never publish private keys or AWS credentials.

## 9. Verification Checklist

| Verification | Expected Result |
|---|---|
| VPC state | Available |
| Four subnets | Created |
| Internet Gateway | Attached to VPC |
| Public route | Routes to Internet Gateway |
| NAT Gateway | Available |
| Private default route | Routes to NAT Gateway |
| Public subnet associations | Correct |
| Private subnet associations | Correct |
| Bastion SSH connectivity | Successful |
| Private EC2 connectivity | Successful |
| Private EC2 outbound access | Verify separately |

## 10. Actual Configuration Details

Complete these fields using verified AWS console values:

- VPC ID:
- VPC IPv4 CIDR:
- Public Subnet A ID and CIDR:
- Public Subnet B ID and CIDR:
- Private Subnet A ID and CIDR:
- Private Subnet B ID and CIDR:
- Internet Gateway ID:
- NAT Gateway ID:
- Public Route Table ID:
- Private Route Table ID:

Do not publish sensitive information such as passwords, private keys, or access credentials.

## 11. Related Documentation

- [Project Overview](../../README.md)
- [AWS Architecture](../../architecture/aws-architecture.md)
- [Screenshots and Evidence](../../screenshots/README.md)
- [Troubleshooting](../../troubleshooting/README.md)
