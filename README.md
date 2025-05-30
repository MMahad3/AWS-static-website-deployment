# 🚀 AWS Static Website Hosting with S3 + CloudFront + HTTPS

This project demonstrates how to host a secure, public static website using:

- ✅ Amazon S3 (for file storage)
- ✅ Amazon CloudFront (CDN + HTTPS)
- ✅ Origin Access Control (OAC) — secure access to S3
- ✅ No custom domain or SSL certificate required

Ideal for learning real-world DevOps skills and AWS services.

---

## 📁 Project Structure

```bash
.
├── index.html                # Main web page
├── index.css                 # CSS styling
├── index.js                  # Optional JavaScript
├── s3-bucket-policy.json     # S3 policy for CloudFront OAC access
├── cloudfront-setup.md       # Step-by-step deployment guide
└── deploy-notes.txt          # Notes, issues, and fixes during setup
