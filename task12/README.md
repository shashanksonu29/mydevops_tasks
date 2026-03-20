
# ⚡ Event-Driven Automation using S3, Lambda & CloudWatch

---

## 🧠 Project Overview

This project demonstrates a **serverless event-driven architecture** on AWS.
Whenever a file is uploaded to an S3 bucket, it automatically triggers a Lambda function, and execution logs are stored in CloudWatch.

### ✅ Key Benefits

* No server management
* Fully automated workflow
* Real-time processing
* Scalable and cost-effective

---

## 🏗️ Architecture

```
User Upload → S3 Bucket → Lambda Trigger → CloudWatch Logs
```

---

## ⚙️ AWS Services Used

| Service    | Purpose                                |
| ---------- | -------------------------------------- |
| Amazon S3  | Stores files and triggers events       |
| AWS Lambda | Processes uploaded files automatically |
| IAM        | Manages permissions securely           |
| CloudWatch | Logs and monitors execution            |

---

## 📌 Prerequisites

Before starting, ensure you have:

* AWS Account
* Basic knowledge of AWS Console
* IAM permissions for S3, Lambda, and CloudWatch
* AWS CLI (optional)

---

## 🪜 Step-by-Step Implementation

---

### 1️⃣ Create S3 Bucket

1. Go to **AWS Console → S3**
2. Click **Create bucket**

**Configuration:**

* Bucket Name: `my-event-bucket-123` *(must be unique)*
* Region: Preferred region (e.g., ap-south-1)
* Keep other settings default

👉 Click **Create bucket**

---

### 2️⃣ Create Lambda Function

1. Go to **AWS Console → Lambda**
2. Click **Create function**
3. Choose **Author from scratch**

**Configuration:**

* Function name: `processS3Upload`
* Runtime: Python 3.9

👉 Click **Create function**

---

### 3️⃣ Configure IAM Permissions

1. Inside Lambda → Go to **Configuration → Permissions**
2. Click on the **Execution Role**
3. Click **Add permissions → Attach policies**

Attach:

* `AWSLambdaBasicExecutionRole`
* `AmazonS3ReadOnlyAccess`

👉 Save changes

---

### 4️⃣ Add Lambda Code

Go to **Code section** and replace with:

```python
import json

def lambda_handler(event, context):
    print("=== FULL EVENT PAYLOAD ===")
    print(json.dumps(event, indent=2))

    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        size = record['s3']['object']['size']

        print(f"File uploaded: {key} ({size} bytes) in bucket {bucket}")

    return {"status": "processed"}
```

👉 Click **Deploy**

---

### 5️⃣ Configure S3 Trigger

1. Go to **S3 → Your Bucket**
2. Open **Properties**
3. Scroll to **Event notifications**
4. Click **Create event notification**

**Configuration:**

* Event Name: `s3-upload-trigger`
* Event Type: **All object create events**
* Destination: **Lambda Function**
* Select: `processS3Upload`

👉 Click **Save**

---

### 6️⃣ Test the Setup

Upload a file to trigger the workflow.

#### Option 1: AWS Console

* Open S3 bucket
* Click **Upload**
* Upload any file

#### Option 2: AWS CLI

```bash
aws s3 cp sample.txt s3://my-event-bucket-123/
```

---

### 7️⃣ Verify in CloudWatch Logs

1. Go to **CloudWatch**
2. Navigate to **Logs → Log groups**
3. Open:

```
/aws/lambda/processS3Upload
```

4. Select the latest log stream

---

### ✅ Expected Logs Output

* Full event payload
* File name
* File size
* Bucket name

---

## 🎯 Expected Outcome

| Action          | Result                       |
| --------------- | ---------------------------- |
| Upload file     | S3 triggers event            |
| Lambda executes | Processes file automatically |
| Logs generated  | Stored in CloudWatch         |

---

## 🚀 Use Cases

* File processing pipelines
* Image/video processing
* Data ingestion workflows
* Event-driven automation
* CI/CD triggers

---

## 🔐 Best Practices

* Follow **least privilege IAM principle**
* Avoid `AdministratorAccess`
* Restrict S3 trigger to specific Lambda
* Enable CloudWatch logging

---

## 🚀 Final Flow

```
User Uploads File → S3 Event → Lambda Execution → CloudWatch Logs
```

✔ Fully automated
✔ Serverless architecture
✔ Scalable and secure

---

## 👨‍💻 Author

**Shashank**
DevOps Engineer | Cloud Enthusiast

