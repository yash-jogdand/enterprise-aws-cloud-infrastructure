
# Amazon RDS and Database Architecture

## 1. Overview

This document describes the planned database layer for the Enterprise AWS Cloud Infrastructure project.

Amazon Relational Database Service (Amazon RDS) will provide a managed relational database for the application tier. MySQL is the planned database engine.

**Implementation status: Planned — not yet deployed or verified.**

## 2. Database Architecture

The planned design places the database in private subnets within the enterprise VPC.

Application EC2 instances will connect to the database using its private endpoint and the configured MySQL port.

The database will not be directly exposed to the public internet.

### Planned Traffic Flow

1. A client sends a request to the Application Load Balancer.
2. The ALB forwards the request to a healthy application EC2 instance.
3. The application connects to the RDS endpoint over the private network.
4. RDS processes the database request.
5. The application returns the response through the ALB.

## 3. Planned RDS Configuration

| Property | Planned Configuration |
|---|---|
| AWS Service | Amazon RDS |
| Database Engine | MySQL |
| VPC | enterprise |
| Subnet Placement | Private subnets |
| Public Accessibility | No |
| Database Port | TCP 3306 |
| Security Group | Dedicated database security group |
| Multi-AZ Deployment | To be evaluated |
| Automated Backups | To be configured |
| Encryption at Rest | To be configured and verified |
| Status | Planned |

Final settings will be recorded after deployment and verification.

## 4. Database Subnet Group

An RDS DB subnet group identifies the subnets in which RDS can place database resources.

Planned requirements:

- Use private subnets in separate Availability Zones.
- Confirm the subnets belong to the enterprise VPC.
- Ensure routing and security settings support application connectivity.
- Select an appropriate subnet group during database creation.

## 5. Database Security Group

The database security group should permit inbound MySQL traffic only from the application tier's security group.

| Direction | Protocol | Port | Source |
|---|---|---|---|
| Inbound | TCP | 3306 | Application Security Group |
| Other inbound traffic | As required | As required | Deny unless explicitly needed |

Do not use `0.0.0.0/0` as the database inbound source.

Application instances should connect using the RDS endpoint, not a hard-coded database IP address.

## 6. Application-to-Database Connectivity

After deployment, connectivity will be tested from an authorized application server.

Verification checklist:

- RDS instance is available.
- RDS endpoint resolves correctly from the application network.
- Database security group permits traffic from the application security group.
- Route tables and network ACLs allow the connection.
- Database credentials are securely configured.
- The application can establish a MySQL connection.

A successful EC2-to-RDS network connection does not by itself prove that database authentication or application queries work. Those must be tested separately.

## 7. Backup and Recovery

Planned database protection measures:

- Configure an appropriate automated backup retention period.
- Verify backup status.
- Evaluate manual snapshots before major changes.
- Document the recovery procedure.
- Test restoration where practical.

Backup and recovery settings will be documented after implementation.

## 8. Availability and Resilience

Potential improvements include:

- Multi-AZ deployment where appropriate.
- Monitoring database health and storage capacity.
- Reviewing database connection limits.
- Testing recovery procedures.
- Evaluating read replicas if application requirements justify them.

Multi-AZ availability and read replicas serve different purposes and should be selected according to availability, recovery and read-scaling requirements.

## 9. Troubleshooting Scenarios

### Application Cannot Connect to RDS

Investigate:

1. RDS instance status.
2. Correct RDS endpoint and port.
3. Database security group inbound rules.
4. Application security group identity.
5. VPC and subnet placement.
6. DNS resolution.
7. Network ACLs and routing.
8. Database credentials and authentication.

### Connection Timeout

Possible causes include blocked security group rules, incorrect routing, network ACL restrictions or an unavailable endpoint.

### Authentication Failure

Check the database username, password, authentication settings and database permissions. A network timeout and an authentication error indicate different classes of problems.

## 10. Implementation Status

| Task | Status |
|---|---|
| Design private database placement | Planned |
| Create RDS DB subnet group | Pending |
| Create database security group | Pending |
| Deploy RDS MySQL | Pending |
| Configure database credentials securely | Pending |
| Test application-to-RDS connectivity | Pending |
| Configure and verify backups | Pending |
| Document actual configuration | Pending |

## 11. Related Documentation

- [Project Overview](../../README.md)
- [AWS Architecture](../../architecture/aws-architecture.md)
- [Networking](../networking/README.md)
- [Compute](../compute/README.md)
- [Load Balancing and Auto Scaling](../load-balancing/README.md)
- [Screenshots and Evidence](../../screenshots/README.md)
- [Troubleshooting](../../troubleshooting/README.md)
