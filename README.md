# cloudlaunch-aws-infrastructure
CloudLaunch AWS Infrastructure is a hands-on cloud engineering project showcasing how to provision, configure, and secure AWS resources for a production-like deployment.
# CloudLaunch AWS Deployment

This repository contains the setup steps and configurations for **CloudLaunch**, covering:

- **Task 1:** Deploy a static website to Amazon S3 with IAM-based access control.
- **Task 2:** Design a secure AWS VPC environment with proper subnet separation and security group rules.

---

## 📌 Task 1 – S3 Static Website Hosting

### Objective
Host a static website on **Amazon S3** with a dedicated IAM user (`cloudlaunch-user`) having **read-only** access to the bucket.

### Steps Taken

1. **Created Three S3 Bucket**

   By clicking through S3 console;

BUCKET NAMES: cloudlaunch-site-bucket-85 ; cloudlaunch-private-bucket-10 ; and cloudlaunch-visible-only-bucket-25

**Enabled Static Website Hosting in the AWS console**

Uploaded index.html and error.html

Configured Bucket Policy:

json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::cloudlaunch-site-bucket-85/*"
    }
  ]
}


**Created IAM User**

Username: ofoncloudlaunch-user

Assigned custom read-only policy for this bucket only.

**IAM Policy JSON**


{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::cloudlaunch-site-bucket-85",
        "arn:aws:s3:::cloudlaunch-site-bucket-85/*"
      ]
    }
  ]
}

**Tested Access**

Verified via: aws s3 ls s3://cloudlaunch-site-bucket-85 --profile ofoncloudlaunch-user

**🌐 S3 Static Website Link**

http://cloudlaunch-site-bucket-85.s3-website-us-east-1.amazonaws.com

📌 Task 2 – VPC Design for CloudLaunch Environment
Objective
Create a secure and logically separated AWS VPC for future expansion.

VPC Details

VPC Name: cloudlaunch-vpc

CIDR Block: 10.0.0.0/16

**Subnets**
| Name                 | CIDR Block    | Type    | Purpose                                |
| -------------------- | ------------- | ------- | -------------------------------------- |
| `cloudlaunch-public` | `10.0.1.0/24` | Public  | Load balancers, public-facing services |
| `cloudlaunch-app`    | `10.0.2.0/24` | Private | Application servers                    |
| `cloudlaunch-db`     | `10.0.3.0/28` | Private | RDS / Database services                |

**Internet Gateway**

Name: cloudlaunch-igw

Attached to cloudlaunch-vpc.

**Route Tables**

Public RT: cloudlaunch-public-rt

Route 0.0.0.0/0 → IGW.

Associated with public subnet only.

Private RTs: cloudlaunch-app-rt & cloudlaunch-db-rt

No route to internet.

Isolated from IGW.

**Security Groups**

cloudlaunch-app-sg – HTTP (80) allowed within VPC only.

cloudlaunch-db-sg – MySQL (3306) allowed only from app subnet.

🛡️ **IAM Permissions for VPC**

Attached policy to cloudlaunch-user for VPC Read-Only:

json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeRouteTables",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeInternetGateways"
      ],
      "Resource": "*"
    }
  ]
}


🧾 **AWS Account Details**

Console URL: https://477094921093.signin.aws.amazon.com/console

Account ID: 477094921093

IAM User: ofoncloudlaunch-user

Password: Ofonime25#

Enforce Change Password on First Login: ✅ Enabled


✅ **Testing

Verified S3 website loads in browser.

Confirmed IAM user cannot upload/delete files.

Confirmed VPC subnets are isolated as per requirements.


**Author**: OFONIME OFFONG

**Date**: 2025-08-11



