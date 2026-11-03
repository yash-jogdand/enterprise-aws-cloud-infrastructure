
# AWS Monitoring and Auditing Documentation

## 1. Overview

This document describes the monitoring and auditing architecture planned for the Enterprise AWS Cloud Infrastructure project.

The primary services covered are:

- Amazon CloudWatch
- AWS CloudTrail

These services help monitor infrastructure health, investigate operational problems, and maintain visibility into AWS account activity.

**Implementation status: Monitoring and auditing configuration is planned. Deployment and verification are pending.**

## 2. Amazon CloudWatch

### Introduction

Amazon CloudWatch is an AWS monitoring and observability service.

It collects and monitors metrics, logs, and events from AWS resources and applications.

### Planned Project Usage

CloudWatch will be used to monitor:

- EC2 instance CPU utilization.
- EC2 instance status checks.
- Application Load Balancer request metrics.
- Application server logs.
- Auto Scaling activity and performance.
- Infrastructure health and availability.

### Important Features

| Feature | Purpose |
|---|---|
| Metrics | Monitor numerical measurements |
| Logs | Collect and inspect log data |
| Alarms | Trigger actions when thresholds are breached |
| Dashboards | Display monitoring information |
| Log Insights | Query and analyze log data |
| Events and integrations | Support event-driven monitoring workflows |

### Planned EC2 Monitoring

The project will evaluate these metrics:

- CPUUtilization
- NetworkIn
- NetworkOut
- StatusCheckFailed
- StatusCheckFailed_Instance
- StatusCheckFailed_System

### Planned ALB Monitoring

The project will evaluate:

- RequestCount
- TargetResponseTime
- HTTPCode_ELB_5XX_Count
- HTTPCode_Target_5XX_Count
- HealthyHostCount
- UnHealthyHostCount

### Planned CloudWatch Alarms

| Alarm | Condition to Evaluate | Purpose |
|---|---|---|
| High CPU | CPU above 80% | Identify high resource usage |
| Unhealthy Targets | Healthy target count below expected | Detect application availability issues |
| EC2 Status Check | Status check failure | Identify instance problems |

Thresholds are proposed examples and must be adjusted to actual workload requirements.

### Planned CloudWatch Dashboard

A dashboard may include:

- EC2 CPU utilization.
- EC2 network activity.
- ALB request count.
- ALB target health.
- Application response time.
- Auto Scaling group capacity.

### Application Log Monitoring

Application logs may be collected using the CloudWatch Agent.

Potential log sources include:

```bash
/var/log/messages
/var/log/httpd/access_log
/var/log/httpd/error_log
```

The actual log paths depend on the operating system and application configuration.

### Planned Verification

- Configure the CloudWatch Agent if required.
- Configure an appropriate IAM role.
- Collect application logs.
- Verify metric availability.
- Create a CloudWatch dashboard.
- Create and test an alarm.
- Verify alarm state transitions.

## 3. AWS CloudTrail

### Introduction

AWS CloudTrail records activity in an AWS account.

It helps identify who performed an action, what action was performed, and when the action occurred.

### Planned Project Usage

CloudTrail will support auditing and investigation of:

- EC2 instance creation and termination.
- Security group modifications.
- VPC and networking changes.
- IAM policy and role changes.
- Load Balancer configuration changes.
- S3 bucket configuration changes.
- Other supported AWS API activity.

### Important Features

- Event history.
- Management events.
- Data events.
- Trail configuration.
- Log file delivery to S3.
- Integration with other AWS monitoring and security services.

### Management Events

Management events record control-plane operations, such as:

- RunInstances
- TerminateInstances
- CreateSecurityGroup
- AuthorizeSecurityGroupIngress
- CreateBucket
- Modify network configuration

Event names and availability depend on the operation and AWS service.

### Data Events

Data events can provide visibility into supported resource-level operations.

Examples include:

- S3 object-level activity.
- Supported Lambda function invocation activity.

Data events may incur additional charges depending on configuration and usage.

### Planned CloudTrail Configuration

| Property | Planned Setting |
|---|---|
| Service | AWS CloudTrail |
| Region | ap-south-1 |
| Trail | To be configured |
| Management Events | To be evaluated |
| Data Events | To be evaluated |
| Log Destination | S3 bucket |
| Status | Planned |

### Planned Verification

- Review CloudTrail Event History.
- Identify recent EC2 API activity.
- Inspect a security group modification event if available.
- Evaluate trail configuration.
- Configure an S3 destination if required.
- Verify log delivery and permissions.

## 4. CloudWatch vs CloudTrail

| Feature | CloudWatch | CloudTrail |
|---|---|---|
| Primary Purpose | Monitoring and observability | Auditing and activity tracking |
| Metrics | Yes | No |
| Application Logs | Yes | No, not as its primary purpose |
| API Activity | Not its primary purpose | Yes |
| Alarms | Yes | Not directly its primary function |
| Dashboards | Yes | No built-in CloudWatch-style dashboards |
| Typical Use | Detect performance problems | Investigate account activity |

## 5. Troubleshooting Scenarios

### CloudWatch Metrics Not Appearing

Investigate:

1. Correct AWS region.
2. Correct metric namespace.
3. Resource and metric availability.
4. IAM permissions.
5. Monitoring configuration.
6. CloudWatch Agent status if using custom metrics or logs.

### CloudWatch Logs Not Appearing

Investigate:

1. CloudWatch Agent installation.
2. Agent configuration.
3. IAM permissions.
4. Log file path.
5. Log group and log stream configuration.
6. Network connectivity.

### CloudWatch Alarm Not Triggering

Investigate:

1. Alarm threshold.
2. Metric namespace and dimensions.
3. Evaluation periods.
4. Datapoints to alarm.
5. Missing data treatment.
6. Alarm state and configuration.

### CloudTrail Events Not Found

Investigate:

1. Correct AWS account.
2. Correct region.
3. Event time range.
4. Event category.
5. Trail configuration.
6. Event history availability and retention.

## 6. Security and Best Practices

- Apply least-privilege IAM permissions.
- Restrict access to CloudWatch logs.
- Protect CloudTrail log storage.
- Enable encryption where appropriate.
- Avoid exposing sensitive application data in logs.
- Review audit events regularly.
- Evaluate log retention and storage costs.
- Use appropriate monitoring thresholds.
- Protect monitoring and auditing configuration from unauthorized changes.

## 7. Implementation Status

| Task | Status |
|---|---|
| Review CloudWatch metrics | Planned |
| Configure application log collection | Pending |
| Create CloudWatch dashboard | Pending |
| Configure CloudWatch alarms | Pending |
| Verify alarm behavior | Pending |
| Review CloudTrail Event History | Pending |
| Configure CloudTrail trail | Pending |
| Configure S3 log destination | Pending |
| Verify CloudTrail log delivery | Pending |

## 8. Future Improvements

- Configure centralized application logging.
- Build operational monitoring dashboards.
- Configure notification workflows for important alarms.
- Evaluate CloudTrail log retention requirements.
- Configure secure audit log storage.
- Review monitoring and auditing costs.
- Establish an operational incident investigation procedure.

## 9. Related Documentation

- [Project Overview](../../README.md)
- [AWS Architecture](../../architecture/aws-architecture.md)
- [Networking](../networking/README.md)
- [Compute](../compute/README.md)
- [Load Balancing](../load-balancing/README.md)
- [Database](../database/README.md)
- [Storage](../storage/README.md)
- [Troubleshooting](../../troubleshooting/README.md)
