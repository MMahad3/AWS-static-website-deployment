# 🌐 AWS CloudFront Setup for Secure Static Website Hosting

This guide explains how to set up a **CloudFront distribution** to serve a static website hosted on **Amazon S3** with **HTTPS** using **Origin Access Control (OAC)**.

---

## ✅ Objectives

- Host a static website using S3
- Serve it securely via CloudFront (HTTPS)
- Block public access to the S3 bucket
- Allow only CloudFront to read from S3

---

## 📦 Prerequisites

- AWS S3 bucket with static site files (e.g., `index.html`, `index.css`, `index.js`)
- Bucket has **static website hosting** enabled
- You have uploaded your site files

---

## 🔐 Step 1: Make S3 Bucket Private

1. Go to **S3 > Your Bucket > Permissions**
2. Click **Edit** under **Block public access**
3. **Enable**: Block all public access ✅
4. Save changes
5. Remove any public bucket policy under **Bucket Policy**

---

## 🔧 Step 2: Create Origin Access Control (OAC)

1. Go to **CloudFront > Security > Origin access control**
2. Click **Create control setting**
3. Fill in:
   - **Name**: `S3-OAC`
   - **Signing behavior**: `Always`
   - **Origin type**: `S3`
4. Click **Create**

---

## 🌍 Step 3: Create CloudFront Distribution

1. Go to **CloudFront > Distributions**
2. Click **Create Distribution**

### 📥 Origin Settings

| Field             | Value                               |
|------------------|-------------------------------------|
| Origin domain     | Select your S3 bucket (.s3.amazonaws.com) |
| Origin path       | Leave empty                         |
| Origin access     | Choose **Origin access control settings** ✅ |
| Origin access control | Select your OAC (e.g., `S3-OAC`) |

> If asked to update the bucket policy, confirm **Yes**.

---

## 👁 Step 4: Viewer Settings

- **Viewer protocol policy**: `Redirect HTTP to HTTPS`
- **Allowed HTTP methods**: `GET, HEAD`
- **Caching policy**: `CachingOptimized`
- **Default root object**: `index.html`

---

## 💸 Step 5: Leave Rest as Default

Unless you need custom config, leave:

- Price class
- Logging
- WAF
- Tags, etc.

Click **Create Distribution**.

---

## 🕒 Step 6: Wait for Deployment

- Status will change to **Deployed**
- You’ll get a URL like:  
  `https://d123456abcdef.cloudfront.net`

Test it in your browser — it should serve your site **with HTTPS**.

---

## 🛡 Step 7: Manually Set S3 Bucket Policy (If Needed)

If CloudFront can’t access your S3 bucket, add this to the **Bucket Policy**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontRead",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::YOUR_ACCOUNT_ID:distribution/YOUR_CLOUDFRONT_ID"
        }
      }
    }
  ]
}
