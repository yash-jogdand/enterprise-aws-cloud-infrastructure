
# AWS Storage Services Documentation

## 1. Overview

This document describes the planned storage architecture for the Enterprise AWS Cloud Infrastructure project.

Three AWS storage services are considered:

- Amazon Elastic Block Store (EBS)
- Amazon Elastic File System (EFS)
- Amazon Simple Storage Service (S3)

Each service addresses a different storage requirement.

**Implementation status: Planned — deployment and integration are pending verification.**

## 2. Amazon EBS

### Introduction

Amazon Elastic Block Store provides persistent block storage volumes for Amazon EC2 instances.

An EBS volume behaves like a virtual disk that can be attached to an EC2 instance.

### Planned Project Usage

- Store application server data.
- Provide persistent block storage for EC2.
- Support application data and operating system volumes.
- Create snapshots for backup and recovery.

### Important Features

- Persistent block storage.
- Multiple volume types.
- Volume resizing and modification.
- Snapshot-based backups.
- Encryption support.
- Availability Zone-specific volumes.

### EBS Volume Types

| Volume Type | Description |
|---|---|
| gp3 | General-purpose SSD |
| gp2 | Previous-generation general-purpose SSD |
| io2 | High-performance provisioned IOPS SSD |
| io1 | Previous-generation provisioned IOPS SSD |
| st1 | Throughput-optimized HDD |
| sc1 | Cold HDD |

### Planned Verification

- Create an EBS volume in the correct Availability Zone.
- Attach it to an EC2 instance.
- Identify the device using Linux commands.
- Create a filesystem if required.
- Mount the volume.
- Verify read and write operations.
- Create and inspect a snapshot.

### Useful Linux Commands

```bash
lsblk
```

```bash
df -h
```

```bash
sudo file -s /dev/xvdf
```

The device name must be verified on the instance. Do not format a volume that contains important existing data.

## 3. Amazon EFS

### Introduction

Amazon Elastic File System is a managed, elastic file storage service.

It supports shared access to files from multiple EC2 instances using the NFS protocol.

### Planned Project Usage

EFS may be used to provide shared application file storage across multiple application servers.

For example, both application servers could access a common directory for shared files.

### Important Features

- Shared file access.
- Elastic storage capacity.
- NFS protocol support.
- Mount targets in Availability Zones.
- Integration with VPC networking.
- Encryption options.
- IAM and network-based access controls.

### Planned Architecture

Application Server A ──┐
                       ├── Amazon EFS
Application Server B ──┘

### Planned Verification

- Create an EFS file system.
- Configure mount targets in the required Availability Zones.
- Configure the EFS security group.
- Permit NFS traffic on TCP port 2049 from the application security group.
- Install the appropriate NFS client.
- Mount EFS on both EC2 instances.
- Create a test file on one instance.
- Verify that the file is accessible from the other instance.

### Useful Linux Commands

```bash
sudo mkdir -p /mnt/efs
```

```bash
df -h
```

```bash
mount | grep efs
```

## 4. Amazon S3

### Introduction

Amazon Simple Storage Service is an object storage service used to store and retrieve data through buckets.

### Planned Project Usage

S3 may be used for:

- Application file storage.
- Backup files.
- Static assets.
- Log archives.
- Infrastructure documentation and deployment artifacts.

### Important Features

- Object storage.
- High durability.
- Versioning.
- Lifecycle management.
- Encryption.
- Bucket policies.
- IAM-based access control.
- Replication options.

### Planned Configuration

| Property | Planned Setting |
|---|---|
| Service | Amazon S3 |
| Bucket | To be created |
| Region | ap-south-1 |
| Public Access | Blocked unless explicitly required |
| Versioning | To be evaluated |
| Encryption | To be configured |
| Lifecycle Rules | To be evaluated |
| Status | Planned |

### Planned Verification

- Create an S3 bucket.
- Configure appropriate public access blocking.
- Configure encryption.
- Upload a test object.
- Verify object retrieval.
- Test IAM or bucket policy permissions.
- Evaluate versioning and lifecycle policies.

## 5. Comparison of EBS, EFS and S3

| Feature | EBS | EFS | S3 |
|---|---|---|---|
| Storage Type | Block | File | Object |
| Main Use | EC2 disks | Shared files | Objects and backups |
| Access | EC2 attachment | Multiple clients | API/HTTP |
| Linux Mount | Yes | Yes | Not natively as a standard filesystem |
| Multi-instance Sharing | Limited to supported configurations | Yes | Via object API |
| Primary Interface | Block device | NFS | S3 API |

## 6. Storage Security

- Enable encryption where appropriate.
- Apply least-privilege IAM permissions.
- Restrict EFS access using security groups.
- Block unintended public S3 access.
- Protect backups and snapshots.
- Avoid publishing sensitive data in the repository.

## 7. Troubleshooting Scenarios

### EBS Volume Not Mounting

Investigate:
1. Volume state and attachment.
2. Availability Zone compatibility.
3. Linux block device name.
4. Filesystem availability.
5. Mount command and filesystem errors.

### EFS Mount Failure

Investigate:
1. EFS file system status.
2. Mount target availability.
3. Security Group rules for TCP 2049.
4. NFS client installation.
5. DNS and network connectivity.

### S3 Access Denied

Investigate:
1. IAM identity policies.
2. Bucket policies.
3. Block Public Access settings.
4. Object ownership and permissions.
5. Encryption and KMS permissions where applicable.

## 8. Implementation Status

| Task | Status |
|---|---|
| Review EBS requirements | Planned |
| Create and mount EBS volume | Pending |
| Verify EBS snapshot | Pending |
| Create EFS file system | Pending |
| Configure EFS mount targets | Pending |
| Mount EFS on both EC2 servers | Pending |
| Create S3 bucket | Pending |
| Configure bucket security | Pending |
| Test S3 upload and retrieval | Pending |

## 9. Future Improvements

- Configure automated backup procedures.
- Evaluate EBS snapshot lifecycle management.
- Test EFS shared storage across both application servers.
- Configure S3 versioning.
- Implement S3 lifecycle policies.
- Review storage costs and retention requirements.

## 10. Related Documentation

- [Project Overview](../../README.md)
- [AWS Architecture](../../architecture/aws-architecture.md)
- [Networking](../networking/README.md)
- [Compute](../compute/README.md)
- [Database](../database/README.md)
- [Screenshots and Evidence](../../screenshots/README.md)
- [Troubleshooting](../../troubleshooting/README.md)
