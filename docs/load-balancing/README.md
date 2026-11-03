
# AWS Application Load Balancer and Auto Scaling Documentation

## 1. Overview

This document describes the load balancing and compute scaling components of the Enterprise AWS Cloud Infrastructure project.

The project uses an Application Load Balancer (ALB) to distribute incoming HTTP requests across private EC2 application servers.

An Auto Scaling Group (ASG) is being configured to manage EC2 application capacity.

## 2. Application Load Balancer

### Purpose

The Application Load Balancer operates at Layer 7 of the OSI model and distributes HTTP/HTTPS traffic across registered targets.

Benefits:
- Distributes application traffic.
- Performs health checks on registered targets.
- Routes requests to healthy application instances.
- Supports highly available application architectures.

### Current Configuration

| Property | Configuration |
|---|---|
| AWS Service | Application Load Balancer |
| VPC | enterprise |
| Scheme | Record actual scheme |
| Listener | HTTP |
| Listener Port | 80 |
| Target Group | alb-target |
| Target Protocol | HTTP |
| Target Port | 80 |
| Health Check Path | / |
| ALB State | Active |

## 3. Target Group Configuration

The target group named `alb-target` contains the private EC2 application servers.

### Health Check Configuration

| Property | Configuration |
|---|---|
| Protocol | HTTP |
| Traffic Port | 80 |
| Health Check Path | / |
| Healthy Targets | Both application servers |
| Health Status | Healthy |

The health check determines whether a registered target can receive application traffic.

### Verification

- Both application servers were registered.
- Both targets reported healthy status.
- ALB browser access was successful.
- The application returned the expected response.
- Repeated PowerShell HTTP requests showed responses from both application servers.

## 4. Application Traffic Flow

1. A client sends an HTTP request.
2. The request reaches the Application Load Balancer.
3. The ALB evaluates the registered target health.
4. The request is forwarded to a healthy EC2 application server.
5. Apache processes the request.
6. The response returns through the ALB.

## 5. Load Balancer Security

Recommended security configuration:

- Allow HTTP or HTTPS traffic to the ALB as required.
- Restrict EC2 application inbound traffic to the ALB Security Group.
- Avoid exposing private EC2 instances directly to the internet.
- Use HTTPS and an appropriate TLS certificate for production deployments.
- Monitor target health and ALB metrics.

## 6. Auto Scaling Group

### Purpose

An Auto Scaling Group manages a collection of EC2 instances and maintains the configured capacity.

It can:
- Launch instances.
- Replace unhealthy instances.
- Maintain desired capacity.
- Scale out when demand increases.
- Scale in when demand decreases.

### Current Implementation Status

| Component | Status |
|---|---|
| EC2 Launch Template | Created |
| Launch Template Version 2 | Created |
| New EC2 Instance | Running and checked |
| Application Security Group | app-sg |
| Target Group Health Check | HTTP port 80, path / |
| Auto Scaling Group Update | Pending verification |

Creating Launch Template version 2 does not automatically update an existing Auto Scaling Group.

## 7. Planned Auto Scaling Configuration

The following items must be verified before declaring the ASG complete:

- Correct Launch Template and version selected.
- Minimum, desired and maximum capacity configured.
- Two private subnets selected where appropriate.
- Correct application Security Group attached.
- Correct target group associated.
- EC2 instances launch successfully.
- New instances pass ALB health checks.
- Instance replacement works correctly.
- Scaling policies behave as expected.

## 8. Testing and Validation

### Completed ALB Tests

| Test | Result |
|---|---|
| ALB state | Active |
| HTTP listener | Configured |
| Target Group | alb-target |
| Application Server A health | Healthy |
| Application Server B health | Healthy |
| Browser access through ALB | Successful |
| Repeated HTTP requests | Responses from both servers |

### Pending ASG Tests

- Verify the ASG uses Launch Template version 2.
- Confirm instance count and desired capacity.
- Verify instance health.
- Test controlled instance replacement.
- Test scaling behavior.
- Confirm newly launched instances register as healthy ALB targets.

## 9. Troubleshooting Scenarios

### ALB Target Unhealthy

Possible investigation steps:
1. Check EC2 instance state.
2. Check Security Group inbound rules.
3. Verify Apache is running.
4. Verify the target group protocol and port.
5. Verify the health check path.
6. Check the application server response.

### HTTP 503 Service Unavailable

Investigate:
- Whether healthy targets are registered.
- Whether the listener forwards to the correct target group.
- Whether the application servers are responding.
- Whether security rules permit ALB-to-EC2 traffic.

### Auto Scaling Not Launching Instances

Investigate:
- Launch Template version.
- Instance type and AMI compatibility.
- Subnet availability.
- Security Group configuration.
- IAM instance profile if required.
- Auto Scaling activity history.
- EC2 service quotas and capacity errors.

## 10. Future Improvements

- Complete the ASG Launch Template update.
- Configure target tracking scaling policies.
- Test automatic unhealthy instance replacement.
- Enable HTTPS using AWS Certificate Manager.
- Configure CloudWatch alarms.
- Review cross-AZ load balancing and resilience.

## 11. Related Documentation

- [Project Overview](../../README.md)
- [AWS Architecture](../../architecture/aws-architecture.md)
- [Networking](../networking/README.md)
- [Compute](../compute/README.md)
- [Screenshots and Evidence](../../screenshots/README.md)
- [Troubleshooting](../../troubleshooting/README.md)
