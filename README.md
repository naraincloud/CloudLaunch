# ☁️ CloudLaunch AWS Deployment

## Overview
**CloudLaunch** is a lightweight AWS-based environment that demonstrates fundamental AWS services — **S3**, **IAM**, and **VPC** — for hosting a basic company website and securely managing internal resources.

This deployment includes:
- A public static website hosted on **Amazon S3**
- A private bucket for restricted internal file storage
- A secure **VPC** with properly segmented subnets and route configurations
- Strict IAM-based access control following the principle of least privilege

---

## 🪣 Task 1 – Static Website Hosting with S3 + IAM

### 🔹 S3 Buckets

| Bucket Name | Description | Access Level |
|--------------|-------------|---------------|
| **cloudlaunch-site-bucket** | Hosts a public static website (HTML/CSS/JS) | Public read-only |
| **cloudlaunch-private-bucket** | Stores private internal files | Private (IAM-controlled) |
| **cloudlaunch-visible-only-bucket** | Appears in S3 bucket list but objects are not accessible | Visible only (no object access) |

### ✅ Static Website Setup
- Static hosting enabled on **cloudlaunch-site-bucket**
- Website files (e.g., `index.html`, `style.css`) uploaded to the bucket
- Public access provided using a minimal read-only bucket policy

**Static Website URL:**
http://cloudlaunch-site-bucket.s3-website-us-east-1.amazonaws.com


---

### 👤 IAM User Configuration

**IAM User:** `cloudlaunch-user`

**Permissions Overview:**

| Permission | Bucket | Description |
|-------------|---------|--------------|
| `s3:ListBucket` | All 3 buckets | Can list bucket names |
| `s3:GetObject`, `s3:PutObject` | `cloudlaunch-private-bucket` | Can upload and download (no delete) |
| `s3:GetObject` | `cloudlaunch-site-bucket` | Read-only access |
| — | `cloudlaunch-visible-only-bucket` | Cannot read object contents |
| `ec2:Describe*` | VPC components | Read-only visibility into networking setup |

**No delete permissions anywhere.**  
**No full admin rights.**

---

### 📄 IAM Policy JSON

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListBuckets",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    },
    {
      "Sid": "AccessCloudLaunchBuckets",
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::cloudlaunch-site-bucket",
        "arn:aws:s3:::cloudlaunch-private-bucket",
        "arn:aws:s3:::cloudlaunch-visible-only-bucket"
      ]
    },
    {
      "Sid": "SiteReadOnly",
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::cloudlaunch-site-bucket/*"
    },
    {
      "Sid": "PrivateReadWrite",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::cloudlaunch-private-bucket/*"
    },
    {
      "Sid": "VisibleOnlyList",
      "Effect": "Deny",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::cloudlaunch-visible-only-bucket/*"
    },
    {
      "Sid": "VPCReadOnly",
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
