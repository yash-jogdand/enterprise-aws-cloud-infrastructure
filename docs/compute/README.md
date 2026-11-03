
# AWS EC2 and Compute Documentation

## 1. Overview

This document describes the compute infrastructure implemented in the Enterprise AWS Cloud Infrastructure project.

The compute layer uses Amazon EC2 instances to host the application servers inside private subnets. A Bastion Host provides controlled administrative access to these private instances.

Apache HTTP Server is installed and configured on both application servers.

## 2. Compute Architecture

The project currently includes:

| Resource | Purpose | Status |
|---|---|---|
| Bastion Host | Administrative SSH access | Implemented and tested |
| Private EC2 Server A | Application hosting | Implemented and tested |
| Private EC2 Server B | Application hosting | Implemented and tested |
| Apache HTTP Server | Web application service | Installed and verified |
| Launch Template | EC2 launch configuration | Version 2 created |
| Auto Scaling Group | Automatic instance management | Configuration update pending |

## 3. Bastion Host

The Bastion Host is deployed in a public subnet.

### Purpose

- Provides controlled SSH access to private EC2 instances.
- Acts as an administrative entry point.
- Avoids exposing private application servers directly to the internet.

### Configuration

| Property | Configuration |
|---|---|
| Service | Amazon EC2 |
| Network | Enterprise VPC |
| Subnet | Public Subnet |
| Public IP | Assigned |
| Access Protocol | SSH |
| SSH Source | Restricted to administrator's IP |

### Verification

- Instance state: Running.
- EC2 status checks: Passed.
- SSH connection: Successful.
- SSH access to private application servers: Successful.

## 4. Private Application Servers

Two EC2 application servers are deployed in private subnets across the VPC's Availability Zones.

### Application Server A

| Property | Configuration |
|---|---|
| Service | Amazon EC2 |
| Network | Enterprise VPC |
| Subnet | Private Subnet A |
| Application | Apache HTTP Server |
| Instance State | Running |
| Status Checks | Passed |
| Apache Service | Active |
| Boot Configuration | Enabled |

### Application Server B

| Property | Configuration |
|---|---|
| Service | Amazon EC2 |
| Network | Enterprise VPC |
| Subnet | Private Subnet B |
| Application | Apache HTTP Server |
| Instance State | Running |
| Status Checks | Passed |
| Apache Service | Active |
| Boot Configuration | Enabled |

Record the actual instance IDs, Availability Zones and instance types after checking the AWS Console.

## 5. SSH Connectivity

Administrative access follows this connection path:

Administrator → Bastion Host → Private EC2 Application Servers

SSH agent forwarding was configured to allow administrative access to the private servers through the Bastion Host.

### Verification

The following operations were successfully performed:

- SSH connection to the Bastion Host.
- SSH connection from Bastion Host to Application Server A.
- SSH connection from Bastion Host to Application Server B.
- SSH agent forwarding verification.

Private SSH keys and credentials must never be uploaded to GitHub.

## 6. Apache Web Server Installation

Apache HTTP Server was installed on both private EC2 instances.

### Linux Commands Used

Check Apache service status:

```bash
sudo systemctl status httpd
```

Start Apache:

```bash
sudo systemctl start httpd
```

Enable Apache at system startup:

```bash
sudo systemctl enable httpd
```

Check local web server response:

```bash
curl http://localhost
```

### Verification Results

| Test | Application Server A | Application Server B |
|---|---|---|
| Apache installed | Passed | Passed |
| Apache service active | Passed | Passed |
| Local HTTP test | Passed | Passed |
| Enabled at boot | Passed | Passed |

Both application servers successfully returned their configured web page through the local HTTP test.

## 7. Application Load Balancer Integration

The EC2 application servers were registered with the Application Load Balancer target group.

### Verification

- Application Load Balancer state: Active.
- Listener configuration: HTTP.
- Target group health checks: Healthy.
- Both application servers registered as healthy targets.
- Browser access through the ALB: Successful.
- Repeated HTTP requests: Responses observed from both application servers.

The ALB distributes incoming application requests across healthy registered targets.

## 8. Launch Template

A Launch Template was created to define the configuration used for launching EC2 instances.

### Current Progress

- Launch Template created.
- New Launch Template version created.
- Latest created version: Version 2.
- Auto Scaling Group update to use the new version: Pending verification.

The existence of a new Launch Template version does not automatically update an existing Auto Scaling Group.

## 9. Auto Scaling Group

The Auto Scaling Group configuration update is a pending implementation task.

Planned verification:

- Confirm the correct Launch Template version.
- Verify minimum, desired and maximum capacity.
- Verify subnet selection across Availability Zones.
- Verify the correct target group association.
- Confirm that launched instances pass health checks.
- Test scaling behavior safely.

Do not mark Auto Scaling as completed until these checks have been performed successfully.

## 10. Security Considerations

- Keep application EC2 instances in private subnets.
- Restrict SSH access to the Bastion Host.
- Avoid opening SSH access to the entire internet.
- Permit application traffic from the appropriate ALB security group.
- Use separate Security Groups for different infrastructure roles where appropriate.
- Avoid storing credentials or private keys in source control.

## 11. Actual Configuration Details

Complete these fields using verified AWS console values:

- Bastion Host Instance ID:
- Application Server A Instance ID:
- Application Server B Instance ID:
- Application Server A Availability Zone:
- Application Server B Availability Zone:
- Application EC2 Instance Type:
- Application Security Group ID:
- ALB Target Group Name:
- Launch Template Name:
- Latest Launch Template Version:

Never publish credentials, private keys or access tokens.

## 12. Future Improvements

- Complete the Auto Scaling Group update.
- Verify automatic instance replacement.
- Test scaling policies.
- Configure CloudWatch monitoring and alarms.
- Integrate application health monitoring.
- Improve automated deployment workflows.

## 13. Related Documentation

- [Project Overview](../../README.md)
- [AWS Architecture](../../architecture/aws-architecture.md)
- [VPC and Networking](../networking/README.md)
- [Screenshots and Evidence](../../screenshots/README.md)
- [Troubleshooting](../../troubleshooting/README.md)
