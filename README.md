> Key Components Summary

**Secure Access Management**
- Implements cross-account IAM roles
- Leverages AWS Systems Manager for secure access
- Least privilege access
- Encrypted communications
- Encryption at rest and in transit

**VM Discovery and Enrollment**
- AWS Systems Manager Fleet Manager for VM management
- CloudWatch agent deployment via Ansible

**Data Collection**
- CloudWatch agent collects disk metrics
- Custom metrics configuration
- 5-minute collection interval
- Centralized dashboard in CloudWatch

**Scalability**
- Horizontal scaling capabilities
- Auto-discovery of new resources
- Multi-region support
  
**Implementation Steps:**
- Deploy cross-account IAM roles
- Install and configure SSM Agent
- Deploy CloudWatch agent
- Configure metrics collection
- Set up alarms and notifications
- Create CloudWatch dashboards
- --------------------------------------------------------------------------------

**Data Flow Steps:**
- CloudWatch Agent collects disk metrics every 5 minutes
- Metrics are sent to CloudWatch Metrics
- Dashboard updates in real-time with new data
- Alarms evaluate metrics against thresholds
- When thresholds are breached, SNS notifications are triggered
- Notifications are distributed to configured endpoints


[EC2 Instances with CloudWatch Agent] --> 
     [Disk Metrics] --> 
[CloudWatch Agent Collection] -->  [CloudWatch Metrics] -->
[CloudWatch Alarms] --> 
     [SNS Topic] -->
    [Notifications - Email/SMS/Slack]

