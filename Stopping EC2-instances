# 🚀 Automating EC2 Start & Stop Using AWS Lambda

## Overview
Manually starting and stopping EC2 instances every day is inefficient and can lead to unnecessary cloud costs.  
This project automates the **start and stop of EC2 instances at fixed times** using **AWS Lambda** and **Amazon EventBridge**, following serverless and least-privilege best practices.

---

## Objective
- Automatically **stop EC2 instances at night**
- Automatically **start EC2 instances in the morning**
- Reduce operational effort and cloud cost
- Build a secure, serverless automation solution

---

## Services Used
- **AWS Lambda** – Executes EC2 start/stop actions
- **Amazon EventBridge** – Time-based scheduling
- **AWS IAM** – Secure access control
- **Amazon SNS** – Notifications for automation events
- **Boto3** – AWS SDK for Python
- **CloudWatch Logs** – Monitoring and debugging

---

## Architecture Overview
1. EventBridge triggers Lambda based on schedule
2. Lambda assumes an IAM role
3. Lambda uses Boto3 to start or stop EC2 instances
4. Execution logs are stored in CloudWatch
5. SNS is used for notification at the service level

---

## Design Decision
Two **separate Lambda functions** are used:
- **Stop EC2 Lambda**
- **Start EC2 Lambda**

### Why separate Lambdas?
- Clear separation of responsibilities
- Easier debugging and maintenance
- Independent scheduling
- Enables least-privilege IAM policies

---

## IAM Configuration

### Trust Policy (Assume Role)
Allows AWS Lambda to assume the role.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "lambda.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
### IAM Policy – Stop EC2 Instances
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StopInstances",
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    }
  ]
}

### IAM Policy – Start EC2 Instances
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    }
  ]
}

---

## Lambda Function – Stop EC2 Instances
import boto3

region = 'ap-south-1'
instances = ['i-xxxxxxxxxxxx', 'i-yyyyyyyyyyyy']

ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print("Stopped instances:", instances)

## Lambda Function – Start EC2 Instances
import boto3

region = 'ap-south-1'
instances = ['i-xxxxxxxxxxxx', 'i-yyyyyyyyyyyy']

ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print("Started instances:", instances)

## Scheduling with Amazon EventBridge
### Stop Schedule (Night)

Triggers Stop Lambda

Example:

9:00 PM IST → cron(30 15 * * ? *)

### Start Schedule (Morning)

Triggers Start Lambda

Example:

8:00 AM IST → cron(30 2 * * ? *)

=> Cron expressions are evaluated in UTC

---

## Amazon SNS (Notification Layer)

Although SNS is not directly invoked inside Lambda, it is used to:

Send notifications when automation runs

Improve visibility and monitoring

Provide execution confirmation

SNS acts as an alerting layer for this automation workflow.

---

## Testing Strategy

Manual Lambda invocation to verify EC2 state changes

Scheduled execution validation

Log verification using CloudWatch Logs

SNS notification confirmation

---

## Monitoring & Logs

CloudWatch captures execution logs automatically

Helps with debugging failures

Confirms scheduled executions

---

## Final Outcome

EC2 instances start and stop automatically

No manual intervention required

Secure IAM setup

Cost optimization achieved

Fully serverless automation

---

## Conclusion

This project demonstrates a real-world use case of AWS serverless automation by combining Lambda, EventBridge, IAM, and SNS.
It highlights how cloud-native services can be orchestrated to create efficient, secure, and cost-optimized solutions.
