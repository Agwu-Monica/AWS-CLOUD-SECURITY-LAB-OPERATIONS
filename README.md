# AWS-CLOUD-SECURITY-LAB-OPERATIONS

This SOP outlines the process for implementing a secure AWS environment using EC2, Systems Manager, Patch Manager, and Config to achieve:

Secure instance management (without SSH)
Continuous compliance monitoring
Automated remediation of security violations
Centralized patch management
🎯 Objectives

By completing this lab, you will:

Manage EC2 instances securely without SSH access
Detect insecure configurations using AWS Config
Automatically remediate security violations
Implement automated patching strategies
Maintain continuous compliance across resources
🔐 Security Principles Applied
Confidentiality: Encrypted storage and restricted access
Integrity: Configuration monitoring and audit trails
Availability: Controlled access and monitoring
IAAA: IAM roles, MFA, least privilege
Cloud Security Operations: Hardening, patching, monitoring
🚀 Implementation Steps
🔹 Step 1: Create and Configure EC2 Instance
Launch an EC2 instance:
Name: Your preferred name
AMI: Amazon Linux
Instance Type: t3.micro
Configure Networking:
Create a VPC
Create 3 subnets
Enable IPv4 and IPv6 CIDR blocks
Enable auto-assign public IP
Configure Security Group:
Allow all traffic (0.0.0.0/0) (for lab purposes only)
Do NOT allow SSH (port 22)
Create a key pair
Launch the instance
Post-launch:
Reboot instance
Attach IAM Role:
Go to Actions → Security → Modify IAM Role
Create and attach a new IAM Role
🔹 Step 2: Enable Systems Manager Access (No SSH)
Navigate to EC2 → Network Interfaces
Select the interface → Manage IP addresses
Enable Auto-assign Public IP
Restart the instance:
Stop → Start
Go to AWS Systems Manager → Fleet Manager
Configure Default Host Management:
Enable Default Host Management Configuration
Create role:
AWSSystemManagerDefaultEC2InstanceManagementRole
Confirm instance appears under Managed Nodes
🔹 Step 3: Connect to Instance via Systems Manager
Go to Fleet Manager → Managed Nodes
Select instance → Node Actions → Connect
Launch terminal session

Run test commands:

whoami
pwd
ls
cd ..
sudo touch onyii
sudo nano security.txt
Validate using:
Node Actions → Tools → File System Viewer
🔹 Step 4: Monitor and Manage Instance

Using Fleet Manager, explore:

📊 Performance Metrics:
CPU Utilization
Disk I/O
Network Traffic
Memory Usage
⚙️ Resource Management:
Processes (CPU & Memory)
Users and Groups
File System
▶️ Run Commands:
Execute remote commands without SSH
🔹 Step 5: Patch Management (Manual)
Go to Systems Manager → Patch Manager
Run Scan:
Patch Operation: Scan
Target: Specific instance
Review compliance:
Patch Manager → Compliance
Create Patch Baseline:
Define approved patches
Set as default if needed
Apply patches:
Operation: Scan and Install
Reboot option: Disabled
🔹 Step 6: Create Patch Policy (Automation)
Navigate to Patch Manager → Patch Policies
Configure:
Patch Operation: Scan
Schedule: Default
Baseline: Default
Create an S3 bucket for logs
Attach S3 bucket to policy
Create patch policy
🔹 Step 7: Create Maintenance Window
Go to Systems Manager → Maintenance Windows
Configure:
Name
Schedule
Duration
Assign tasks (patching, commands)
🔹 Step 8: Enable AWS Config (Compliance Monitoring)

AWS Config tracks resource changes and enforces rules.

Setup:
Navigate to AWS Config → Set Up
Configure:
Resource tracking
S3 bucket for logs
Add Managed Rule:
Rule: restricted-ssh
Scope: EC2 Security Groups
🔹 Step 9: Test Compliance Detection
Modify EC2 Security Group:
Add SSH rule (0.0.0.0/0)
Refresh AWS Config:
Resource becomes Non-compliant
Fix manually:
Remove SSH rule
Confirm compliance restored
🔹 Step 10: Enable Automatic Remediation
1. Create IAM Role
Service: Systems Manager
Role Name: ConfigRemediationRole
Attach Policy:
AmazonSSMAutomationRole
2. Create Custom Policy
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

3. Configure Remediation in AWS Config
Go to AWS Config → Rules → Manage Remediation
Select:
Automatic remediation
Retry settings
Provide:
IAM Role ARN
Security Group ID
Save configuration
4. Validate Automation
Reintroduce violation:
Add SSH rule (0.0.0.0/0)
AWS Config detects violation
Automatic remediation removes rule
✅ Expected Outcomes
EC2 managed without SSH
Centralized control via Systems Manager
Automated patching and maintenance
Real-time compliance monitoring
Automatic remediation of security risks
📌 Key Takeaways
Systems Manager replaces SSH for secure access
Patch Manager ensures systems are up-to-date
AWS Config enforces compliance continuously
Automation reduces human error in security








## Notion Link
https://swamp-diadem-74c.notion.site/AWS-CLOUD-SECURITY-LAB-OPERATIONS-33b5e80cc2358068aed4e471bdd436ce?source=copy_link
