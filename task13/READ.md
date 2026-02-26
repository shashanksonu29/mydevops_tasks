# Hosting a Static Website on AWS using S3 and CloudFront

## Project Overview

This project demonstrates how to host a static website on AWS using:

-   Amazon S3 (Storage)
-   Amazon CloudFront (CDN)
-   Route 53 (Optional -- Custom Domain)
-   CodePipeline (Optional -- CI/CD Automation)

This setup is serverless, scalable, cost-effective, and
production-ready.

------------------------------------------------------------------------

## Architecture

User → CloudFront → S3 (Private Bucket)

S3 bucket remains private. CloudFront serves content securely using
Origin Access Control (OAC).

------------------------------------------------------------------------

# Step 1: Create S3 Bucket

1.  Go to AWS Console → S3 → Create Bucket
2.  Enter a globally unique bucket name
3.  Select Region
4.  Keep:
    -   ACLs Disabled (recommended)
    -   Block All Public Access ENABLED (important for security)
5.  Create bucket

------------------------------------------------------------------------

# Step 2: Enable Static Website Hosting (If Required)

Go to: S3 → Bucket → Properties → Static Website Hosting

-   Enable
-   Index document: index.html
-   Error document: index.html (for React SPA)

------------------------------------------------------------------------

# Step 3: Upload Website Files

Using AWS Console: - Upload index.html, style.css, and other files

Using CLI:

aws s3 sync ./build/ s3://your-bucket-name/ --delete

------------------------------------------------------------------------

# Step 4: Create CloudFront Distribution

1.  Go to CloudFront → Create Distribution
2.  Origin: Select S3 bucket
3.  Origin Access: Create Origin Access Control (OAC)
4.  Viewer Protocol Policy: Redirect HTTP to HTTPS
5.  Default Root Object: index.html
6.  Create Distribution

After creation, update S3 bucket policy to allow CloudFront access.

------------------------------------------------------------------------

# Step 5: Configure Error Handling for SPA (React)

CloudFront → Error Pages → Create Custom Error Response

For: 403 and 404 Response Page Path: /index.html HTTP Response Code: 200

------------------------------------------------------------------------

# Step 6: (Optional) Configure Custom Domain using Route 53

1.  Request SSL Certificate in ACM (us-east-1)
2.  Add Alternate Domain Name in CloudFront
3.  Create A record in Route 53 pointing to CloudFront

------------------------------------------------------------------------

# Step 7: Automate Deployment using CodePipeline

Pipeline Stages:

1.  Source -- GitHub Repository
2.  Build -- CodeBuild (npm install && npm run build)
3.  Deploy -- S3 Bucket
4.  Invalidate CloudFront Cache

Sample invalidation command:

aws cloudfront create-invalidation --distribution-id YOUR_ID --paths
"/\*"

------------------------------------------------------------------------

# Security Best Practices

-   Keep S3 bucket private
-   Use CloudFront OAC
-   Enable HTTPS only
-   Use AWS WAF for protection
-   Enable S3 versioning

------------------------------------------------------------------------

# Expected Output

Website accessible via:

https://dxxxxxxxx.cloudfront.net

or

https://yourdomain.com

Fast, secure, globally distributed, and production-ready.

------------------------------------------------------------------------

# Author

DevOps Implementation Guide
