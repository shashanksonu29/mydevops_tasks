
# 🚀 Host a Static Website on AWS (S3 + CloudFront)

Deploy a fast, secure, and scalable **static website** using AWS services like **Amazon S3** and **CloudFront CDN**.

---

## 📌 Project Overview

This project demonstrates how to host a static website in a **serverless architecture** using:

* **Amazon S3** – Stores static website files
* **Amazon CloudFront** – Delivers content globally with low latency
* **Amazon Route 53 (Optional)** – Configures custom domain

---

## 🏗️ Architecture

```
User → CloudFront (CDN) → S3 Bucket → Static Website Files
```

---

## ✅ Prerequisites

Before you begin, ensure you have:

* AWS Account
* IAM user with S3 & CloudFront permissions
* AWS CLI installed and configured
* GitHub repository cloned

👉 Repo: [https://github.com/vineethsankre/AWS_Codepipeline_S3_CloudFront.git](https://github.com/vineethsankre/AWS_Codepipeline_S3_CloudFront.git)

### Install AWS CLI

```bash
pip install awscli
```

### Configure AWS Credentials

```bash
aws configure
```

---

## 📂 Project Structure

```
website/
├── index.html
├── error.html
├── css/
├── js/
└── images/
```

> For React apps, generate build files:

```bash
npm run build
```

---

## ⚙️ Step-by-Step Setup

### 1️⃣ Create an S3 Bucket

* Navigate to **AWS Console → S3**
* Click **Create bucket**

**Configuration:**

* Bucket Name: `my-static-site-bucket`
* Region: Your preferred region
* Disable **Block all public access**

---

### 2️⃣ Enable Static Website Hosting

* Go to **Bucket → Properties**
* Enable **Static website hosting**

**Set:**

* Index document: `index.html`
* Error document: `error.html`

---

### 3️⃣ Configure Bucket Policy

Navigate to **Permissions → Bucket Policy** and add:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::my-static-site-bucket/*"
    }
  ]
}
```

> 🔁 Replace `my-static-site-bucket` with your actual bucket name.

---

### 4️⃣ Upload Website Files

Upload using AWS Console or CLI:

```bash
aws s3 sync ./website s3://my-static-site-bucket
```

---

### 5️⃣ Create CloudFront Distribution

* Go to **CloudFront → Create Distribution**

**Settings:**

* Origin: Select S3 bucket
* Viewer Protocol Policy: Redirect HTTP → HTTPS
* Default Root Object: `index.html`

⏳ Wait for deployment to complete.

---

### 6️⃣ Access Your Website

CloudFront will generate a URL like:

```
https://dxxxxx.cloudfront.net
```

Open it in your browser 🎉

---

### 7️⃣ (Optional) Configure Custom Domain

Using **Route 53**:

1. Create a Hosted Zone
2. Create an **A Record**
3. Enable **Alias → CloudFront Distribution**

Example:

```
www.example.com → CloudFront
```

🔐 Add SSL certificate via **AWS Certificate Manager (ACM)**

---

## 🔄 Cache Invalidation

After updating your site, clear cache:

```bash
aws cloudfront create-invalidation \
--distribution-id DISTRIBUTION_ID \
--paths "/*"
```

---

## 🎯 Expected Outcome

Your website will be live on:

* ✅ CloudFront URL
* ✅ Custom domain (if configured)

### Key Benefits

* ⚡ Fast global delivery (CDN)
* 🔒 HTTPS enabled
* 📈 Highly scalable & serverless
* 💰 Cost-effective hosting

---

## 🧹 Cleanup

To avoid unnecessary charges:

1. Delete CloudFront Distribution
2. Delete S3 Bucket

---

## 👨‍💻 Author

**Shashank**
DevOps Engineer | Cloud Enthusiast
