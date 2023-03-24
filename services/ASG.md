# Auto Scaling Group (ASG)
A collection of EC2 instances that are treated as a logical grouping for the purposes of automatic scaling and management.


## Useful links
- [AWS - ASG FAQs](https://aws.amazon.com/ec2/autoscaling/faqs/)

## General Notes
- **Scale out** (add EC2 Instance) to match an **increased** load
- **Scale in** (remove EC2 Instance) to match a **decrease** load
- Ensure we have a minimum and a maximum number of EC2 instance running
- Automatically Register new instances to a load balancer
- Re create an EC2 instance in case a previous one is terminated (ex: if unhealthy)
- Free service (only pay for underlying EC2 Instances)
- It is possible to scale based on CloudWatch alarms (ex: Scale when average CPU usage is high)

### ASG Attributes
- Launch Template
  - AMI + Instance type
  - EC2 User Data
  - EBS Volumes
  - Security Groups
  - SSG Key Pair
  - IAM Roles for your EC2 Instances
  - Network + Subnets Information
  - Load Balancer Information
- Min size / Max size / Initial Capacity

## Scaling Policies
- **Dynamic Scaling Policies:**
  - **Target Tracking Scaling**: I want the AVG CPU to stay around 40%
  - **Simple / Step Scaling**: CloudWatch alarm trigger (cpu> 70%) add 2 units and  CloudWatch alarm trigger (cpu < 30%) remove 1 units
  - **Scheduled Actions**: Increase to minimum capacity to 10 at 5pm on Fridays
- **Predictive Scaling**:
  - **Predictive Scaling**: continuously forecast load and schedule scaling ahead. Machine learn powered

### Good metrics to scale on
- **CPU Utilization**
- **RequestCountPerTarget**: Make sure the number of requests per EC2 is stable
- **Average Network in / out**
- **Any custom metric**: using cloudwatch

## Scaling Cooldowns
- After scaling activity happens, you are in the cooldown period (default 300s)
- During this period the ASG will not launch or terminate additional instance (to allow for metrics to stabilize)

-Advice: Use a ready-to-use AMI  to reduce configuration time in order to be serving request faster and reduce the cooldown period


## Instance refresh
- Is a way to update launch template recreating all EC2 Instances
- We must set a minimum healthy percentage and specify warmup time (how long until the instance is ready to use)
- Example: Imagine that we update our launch template (UPDATED AMI) and must replace all ec2 instances in an ASG. So, we configure our instance refresh to recreate all instances keeping the ratio of min healthy percentage in order to gradually replace instances.