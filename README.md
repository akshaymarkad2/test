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

  ┌────────────────────────────────────────────────────────────────┐
│                      EC2 Instances                             │
│  ┌──────────────────────────────────────────────────┐         │
│  │               CloudWatch Agent                    │         │
│  │  ┌────────────────┐    ┌─────────────────┐      │         │
│  │  │ Disk Metrics   │    │  Custom Metrics  │      │         │
│  │  │ - Used Space   │    │  - IOPS         │      │         │
│  │  │ - Free Space   │    │  - Latency      │      │         │
│  │  │ - Utilization %│    │  - Queue Length │      │         │
│  │  └────────────────┘    └─────────────────┘      │         │
│  └──────────────────────────────────────────────────┘         │
└────────────────────────────────┬───────────────────────────────┘
                                 ↓
┌────────────────────────────────────────────────────────────────┐
│                     CloudWatch Metrics                         │
│  ┌──────────────────────────────────────────────────┐         │
│  │              Metric Namespace                     │         │
│  │  - CWAgent/disk_used_percent                     │         │
│  │  - CWAgent/disk_free                             │         │
│  │  - CWAgent/disk_total                            │         │
│  └──────────────────────────────────────────────────┘         │
└───────────────────────────────┬────────────────────────────────┘
                                ↓
┌────────────────────────────────────────────────────────────────┐
│                    CloudWatch Dashboard                        │
│  ┌──────────────────────┐    ┌───────────────────┐           │
│  │   Disk Usage Graph   │    │  Metric Statistics │           │
│  │                      │    │                    │           │
│  │    [Line Graph]      │    │  - Min/Max/Avg     │           │
│  │                      │    │  - Trends          │           │
│  └──────────────────────┘    └───────────────────┘           │
└───────────────────────────────┬────────────────────────────────┘
                                ↓
┌────────────────────────────────────────────────────────────────┐
│                    CloudWatch Alarms                           │
│  ┌─────────────────────────────────────────────────┐          │
│  │  Warning Alarm (75%)  │   Critical Alarm (90%)  │          │
│  │  ┌─────────────────┐ │   ┌─────────────────┐   │          │
│  │  │ Evaluation: 2/3 │ │   │ Evaluation: 2/3 │   │          │
│  │  │ Period: 5 min   │ │   │ Period: 5 min   │   │          │
│  │  └─────────────────┘ │   └─────────────────┘   │          │
│  └─────────────────────────────────────────────────┘          │
└───────────────────────────────┬────────────────────────────────┘
                                ↓
┌────────────────────────────────────────────────────────────────┐
│                         SNS Topic                              │
│  ┌─────────────────────────────────────────────────┐          │
│  │              Message Format                      │          │
│  │  {                                              │          │
│  │    "instanceId": "i-xyz",         │          │
│  │    "diskUsage": "85%",                          │          │
│  │    "mountPoint": "/",                           │          │
│  │    "timestamp": "2024-04-20T10:00:00Z"          │          │
│  │  }                                              │          │
│  └─────────────────────────────────────────────────┘          │
└───────────────────────────────┬────────────────────────────────┘
                                ↓
┌────────────────────────────────────────────────────────────────┐
│                    Notification Endpoints                       │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐     │
│  │    Email     │    │     SMS      │    │    Slack     │     │
│  │ Distribution │    │  Messages    │    │  Channel     │     │
│  │   Lists      │    │             │    │             │     │
│  └──────────────┘    └──────────────┘    └──────────────┘     │
└────────────────────────────────────────────────────────────────┘

