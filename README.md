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
