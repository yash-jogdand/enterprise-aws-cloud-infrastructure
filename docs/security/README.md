
# AWS Security and IAM Documentation

## 1. Overview

This document describes the security architecture and access-control practices for the Enterprise AWS Cloud Infrastructure project.

The objective is to protect AWS resources, restrict unauthorized access, and apply security best practices across networking, compute, storage, and application infrastructure.

Security topics covered:

- AWS Identity and Access Management (IAM)
- IAM users, groups, roles, and policies
- Security groups
- Network ACLs
- Bastion Host access
- EC2 instance access
- Encryption and data protection
- Security verification and troubleshooting

**Implementation status:** Some network-level security controls have been configured. A complete IAM and security-hardening review is pending.

## 2. AWS Identity and Access Management (IAM)

### Introduction

AWS IAM is a service used to manage access to AWS resources.

It controls:

- Who can access AWS resources.
- Which actions users and services can perform.
- Which resources those actions apply to.
- Under what conditions access is allowed.

### Important IAM Components

| Component | Purpose |
|---|---|
| IAM User | Represents an identity requiring AWS access |
| IAM Group | Organizes IAM users with common permissions |
| IAM Role | Provides temporary credentials to trusted entities |
| IAM Policy | Defines allowed or denied actions |
| IAM Identity Center | Centralizes workforce access management |

### IAM Policy Structure

An IAM policy is generally a JSON document containing:

- Effect
- Action
- Resource
- Optional Condition

Example policy structure:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    }
  ]
}
```

This example permits describing EC2 instances. It is illustrative and is not a complete project deployment policy.

### Least Privilege Principle

Least privilege means granting only the permissions required to perform a specific task.

Best practices:

- Avoid unnecessary administrator permissions.
- Use narrowly scoped policies.
- Review permissions regularly.
- Remove unused access.
- Prefer temporary credentials where possible.
- Do not share access keys.

## 3. IAM Roles for EC2

EC2 instances may need permission to communicate with AWS services.

An IAM role attached through an instance profile can provide temporary credentials to an EC2 instance.

Potential project use cases:

- Reading objects from an authorized S3 bucket.
- Publishing application logs to CloudWatch.
- Accessing approved AWS resources.
- Supporting monitoring and operational tasks.

Security recommendations:

- Attach only the required IAM role.
- Avoid storing AWS access keys on EC2 instances.
- Do not grant unrestricted S3 or CloudWatch permissions.
- Review role trust relationships.
- Verify the instance role before enabling application access.

**Status:** EC2 IAM role configuration and permission verification are pending.

## 4. Network Security

### Security Groups

Security groups act as virtual firewalls for supported AWS resources.

They control inbound and outbound traffic.

Project security group examples:

| Security Group | Intended Purpose |
|---|---|
| Bastion security group | Restricted administrative SSH access |
| app-sg | Application server traffic |
| ALB security group | Client HTTP/HTTPS traffic |
| Database security group | Restricted database access |

### Planned Security Group Rules

| Resource | Protocol | Port | Intended Source |
|---|---|---|---|
| Bastion | TCP | 22 | Administrator's public IP |
| Application servers | TCP | 80 | ALB security group |
| Application servers | TCP | 22 | Restricted administrative access through Bastion, if required |
| Database | TCP | 3306 | Application security group |
| ALB | TCP | 80 | Intended client traffic |
| ALB | TCP | 443 | Intended client traffic when HTTPS is configured |

These are intended design rules. Actual configured rules must be verified in the AWS console before being described as implemented.

### Network ACLs

Network ACLs operate at the subnet level.

Important characteristics:

- They support inbound and outbound rules.
- They are stateless.
- They support allow and deny rules.
- Rule evaluation follows rule-number order.

Security groups are stateful, whereas network ACLs are stateless.

### Network Security Best Practices

- Restrict SSH access to trusted administrator IP addresses.
- Avoid unrestricted administrative access.
- Allow application traffic only from intended sources.
- Restrict database access to application resources.
- Review inbound and outbound rules.
- Avoid unnecessarily broad CIDR ranges.

## 5. Bastion Host Security

The Bastion Host provides an administrative entry point to private EC2 instances.

Project implementation:

- Bastion Host deployed in a public subnet.
- Private application servers deployed in private subnets.
- SSH connectivity to the Bastion verified.
- SSH agent forwarding used to connect to both private application servers.
- Bastion SSH source restricted to the administrator's public IP.

### Security Recommendations

- Restrict SSH access.
- Use secure key-pair management.
- Avoid sharing private keys.
- Monitor administrative access.
- Keep the operating system updated.
- Consider Systems Manager Session Manager as an alternative administrative access method.

## 6. Encryption and Data Protection

Encryption protects data from unauthorized access.

### Data at Rest

Potential encryption services and features:

- EBS volume encryption.
- S3 server-side encryption.
- RDS storage encryption.
- EFS encryption at rest.

### Data in Transit

Security measures may include:

- HTTPS using TLS certificates.
- Secure SSH connections.
- Encrypted database connections where configured.
- Secure communication between applications and AWS services.

### AWS Key Management Service

AWS KMS manages cryptographic keys used by supported AWS services.

Potential project usage:

- Encrypting EBS volumes.
- Encrypting S3 objects.
- Encrypting RDS storage.
- Managing encryption keys and permissions.

**Status:** Encryption configuration and verification for future storage and database resources are pending.

## 7. Security Auditing

Security auditing helps identify configuration changes and suspicious activity.

Services relevant to this project:

| Service | Security Purpose |
|---|---|
| AWS CloudTrail | Records AWS account activity |
| Amazon CloudWatch | Monitors infrastructure and application health |
| AWS IAM | Controls access to resources |
| AWS KMS | Manages encryption keys |
| Amazon GuardDuty | Evaluates supported threat-detection findings |

CloudTrail documentation:

[Monitoring and Auditing Documentation](../monitoring/README.md)

## 8. Security Verification Checklist

### IAM

- Review IAM users and groups.
- Review attached policies.
- Identify unnecessary administrator access.
- Review IAM roles and trust policies.
- Verify EC2 instance profiles where required.

### Networking

- Verify Bastion SSH restrictions.
- Review application security group rules.
- Verify database security group restrictions before database deployment.
- Review network ACLs.
- Confirm private application servers do not have unintended public access.

### Storage and Encryption

- Verify EBS encryption when volumes are deployed.
- Review S3 public access settings.
- Verify bucket policies.
- Review encryption configuration.
- Verify RDS and EFS security settings after deployment.

### Auditing

- Review CloudTrail activity.
- Verify log storage permissions.
- Review monitoring alarms.
- Investigate unexpected infrastructure changes.

## 9. Security Troubleshooting

### SSH Connection Failure

Check:

1. Bastion instance status.
2. Public IP address.
3. Security group inbound SSH rule.
4. Administrator public IP restriction.
5. Key pair and file permissions.
6. Network route configuration.

### Private EC2 Connection Failure

Check:

1. Bastion-to-private-instance network connectivity.
2. SSH agent forwarding configuration.
3. Private instance security group.
4. Network ACL rules.
5. Correct private IP address.
6. SSH service availability.

### Application Traffic Blocked

Check:

1. ALB security group inbound rules.
2. Application security group inbound rules.
3. Target group configuration.
4. Application service status.
5. Network ACL rules.
6. Target health status.

### Database Connection Failure

After database deployment, check:

1. Database endpoint.
2. Database security group.
3. Application security group as the permitted source.
4. Database port.
5. Database subnet placement.
6. Network connectivity and database credentials.

## 10. Implementation Status

| Security Task | Status |
|---|---|
| Bastion Host SSH access restricted to administrator IP | Configured |
| Bastion-to-private EC2 SSH connectivity | Verified |
| Application security group | Configured |
| IAM least-privilege review | Pending |
| EC2 IAM roles review | Pending |
| Network ACL security review | Pending |
| EBS encryption verification | Pending |
| S3 security configuration | Pending |
| RDS security configuration | Pending |
| CloudTrail auditing deployment | Pending |
| Security hardening review | Pending |

## 11. Future Security Improvements

- Implement least-privilege IAM policies.
- Review EC2 instance roles.
- Evaluate AWS Systems Manager Session Manager.
- Enable HTTPS using an appropriate TLS certificate.
- Evaluate AWS KMS encryption requirements.
- Enable and verify CloudTrail logging.
- Review S3 public access and bucket policies.
- Evaluate GuardDuty and other security services.
- Establish regular security reviews.

## 12. Related Documentation

- [Project Overview](../../README.md)
- [AWS Architecture](../../architecture/aws-architecture.md)
- [Networking](../networking/README.md)
- [Compute](../compute/README.md)
- [Load Balancing](../load-balancing/README.md)
- [Database](../database/README.md)
- [Storage](../storage/README.md)
- [Monitoring and Auditing](../monitoring/README.md)
- [Troubleshooting](../../troubleshooting/README.md)
