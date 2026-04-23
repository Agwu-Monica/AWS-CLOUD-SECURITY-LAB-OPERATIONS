# AWS-CLOUD-SECURITY-LAB-OPERATIONS

This  is the process for implementing a secure AWS environment using EC2, Systems Manager, Patch Manager, and Config to achieve:

----------------------------------------------------------------------------------------------------------

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/cb24f2db-e575-4586-8dbe-03b8bda71485" />

-----------------------------------------------------------------------

## Architecture Summary

EC2 Instance (Amazon Linux)

VPC with Subnets

IAM Roles

AWS Systems Manager (SSM)

Patch Manager

AWS Config

S3 Bucket (for logs)

------------------------------------------------------------------

## You will learn how to:

Secure instance management (without SSH)

Continuous compliance monitoring

Automated remediation of security violations

Centralized patch management

------------------------------------------------------------------------
## Objectives

By completing this lab, you will:

Manage EC2 instances securely without SSH access

Detect insecure configurations using AWS Config

Automatically remediate security violations

Implement automated patching strategies

Maintain continuous compliance across resources

------------------------------------------------------------------

## Security Principles Applied

Confidentiality: Encrypted storage and restricted access

Integrity: Configuration monitoring and audit trails

Availability: Controlled access and monitoring

IAAA: IAM roles, MFA, least privilege

Cloud Security Operations: Hardening, patching, monitoring

-----------------------------------------------------------------------------

## Implementation Steps

### Step 1: Create and Configure EC2 Instance

Launch an EC2 instance:

Name: Your preferred name

AMI: Amazon Linux

Instance Type: t3.micro

#### Configure Networking:

Create a VPC

Create 3 subnets

Enable IPv4 and IPv6 CIDR blocks

Enable auto-assign public IP

#### Configure Security Group:

Allow all traffic (0.0.0.0/0) (for lab purposes only)

Do NOT allow SSH (port 22) Remove SSH (port 22) → we will use SSM instead

#### Create a key pair

Launch the instance

#### Post-launch:

Reboot instance

#### Attach IAM Role:

Go to Actions → Security → Modify IAM Role

Create and attach a new IAM Role


### Step 2: Enable Systems Manager Access (SSM) (No SSH)

Navigate to EC2 → Network Interfaces

Select the interface

Click Actions → Manage IP Address

Enable Auto-assign Public IP

Restart the instance:

Stop → Start instance


### Go to AWS Systems Manager → Fleet Manager

Click Account Management → Configure

Configure Default Host Management:

Enable Default Host Management Configuration

Create role:
AWSSystemManagerDefaultEC2InstanceManagementRole

Confirm instance appears under Managed Nodes


### Step 3: Connect to Instance via Systems Manager (SSM)

Go to Fleet Manager → Managed Nodes

Select instance → Node Actions → Connect

Launch terminal session

Run Commands (Inside Terminal)

whoami

pwd

ls

cd ..

pwd

ls

Create Files

sudo touch onyii

sudo nano security.txt

Add text:

I love to code

View Files via GUI (Graphical User Interface)

Go to:

Node Actions → Tools → File System Viewer

You can:

View files

Check permissions

Create directories



### Step 4: Monitor and Manage Instance

Inside Fleet Manager, explore:

#### Performance Metrics:

CPU Utilization

Disk I/O

Network Traffic

Memory Usage

#### Resource Management:

Processes (CPU & Memory)

Users and Groups

#### File System

Run Commands:

Execute remote commands without SSH


### Step 5: Patch Management (Manual)

Go to Systems Manager → Patch Manager

Run Scan:

Patch Operation: Scan

Target: Manual Instance Selection

Choose your instance

#### Review compliance:

Patch Manager → Compliance

Create Patch Baseline:

Define approved patches

Set as default if needed

Apply patches:

Operation: Scan and Install

Reboot option: Disabled


### Step 6: Create Patch Policy (Automation)

Navigate to Patch Manager → Patch Policies

Configure:

Patch Operation: Scan

Schedule: Default

Baseline: Default

#### Create an S3 bucket for logs

Open new tab → Search S3

Create bucket (for logs)

Link Bucket

Return to Patch Policy

Select bucket

Click Create



### Step 7: Create Maintenance Window

Go to Systems Manager → Maintenance Windows

Click Create

Configure:

Name

Schedule

Duration (e.g., weekly patching)

Save

Assign tasks (patching, commands)



### Step 8: Enable AWS Config (Compliance Monitoring)

What AWS Config Does

Tracks configuration changes

Detects misconfigurations

Enforces compliance rules

#### Setup:

Search AWS Config

Click Get Started

Configure:

Resource tracking

S3 bucket for logs

#### Add Managed Rule:

Rule: restricted-ssh

Search: restricted-ssh

Rename: restricted-ssh-2

Apply to:
EC2 Security Group



### Step 9: Test Compliance Detection/violation

#### 1. Break the Rule

Go to EC2 → Security Group

Add:
SSH (port 22)
Source: 0.0.0.0/0

#### 2. Check AWS Config

Refresh → shows:

❌ Non-compliant

Fix manually:

Remove SSH rule

Confirm compliance restored


### Step 10: Enable Automatic Remediation

Go to AWS Config Rule

Click Manage Remediation

Select:

Automatic remediation

#####  Create IAM Role

Go to IAM → Roles → Create Role

Service: Systems Manager

Attach:
AmazonSSMAutomationRole

Role Name: ConfigRemediationRole


#### 2. Create Custom Policy

{
  "Version":"2012-10-17",
  "Statement": [
    {
      "Effect":"Allow",
      "Action": [
        "ec2:DescribeSecurityGroups",
        "ec2:RevokeSecurityGroupIngress"
      ],
      "Resource":"*"
    }
  ]
}


Attach this policy to the role.

-------------------------------------------------------------------------------------

## Configure Remediation in AWS Config

Go to AWS Config → Rules → Manage Remediation

Select:

Automatic remediation

Retry settings

Provide:

IAM Role ARN

Security Group ID

Save configuration

--------------------------------------------------------------------------

## Validate Automation

Reintroduce violation:

Add SSH rule (0.0.0.0/0)

AWS Config detects violation

Automatic remediation removes rule

#### ✅ Expected Outcomes

EC2 managed without SSH

Centralized control via Systems Manager

Automated patching and maintenance

Real-time compliance monitoring

Automatic remediation of security risks

---------------------------------------------------------------------

## Key Takeaways

Systems Manager replaces SSH for secure access

Patch Manager ensures systems are up-to-date

AWS Config enforces compliance continuously

Automation reduces human error in security

-----------------------------------------------------------------------

## Notion Link
https://swamp-diadem-74c.notion.site/AWS-CLOUD-SECURITY-LAB-OPERATIONS-33b5e80cc2358068aed4e471bdd436ce?source=copy_link

--------------------------------------------------------------------------

## Author

Monica Agwu

*Cloud Engineering & DevOps

https://www.linkedin.com/in/agwumonica/
