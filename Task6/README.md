13/1/2026
# mydevops_tasks

🌐 AWS Task-6 — Route 53 + ALB + HTTPS Setup

Domain: vineethsworld.xyz

🧭 Objective

Configure a custom domain using Amazon Route 53, route traffic through an Application Load Balancer (ALB), and secure the application with HTTPS using AWS Certificate Manager (ACM).

🌍 Step 1: Create a Route 53 Hosted Zone

Navigate to AWS Console → Route 53 → Hosted Zones

Click Create hosted zone

Provide the following details:

Domain name: vineethsworld.xyz

Type: Public hosted zone

Click Create hosted zone

AWS will generate four Name Servers similar to:

ns-xxx.awsdns-xx.com
ns-xxx.awsdns-xx.net
ns-xxx.awsdns-xx.org
ns-xxx.awsdns-xx.co.uk

🌐 Step 2: Update Nameservers at Domain Registrar

Log in to your domain registrar (GoDaddy, Namecheap, etc.)

Navigate to DNS / Nameservers

Select Custom nameservers

Replace existing values with the four AWS nameservers

Save changes

⏳ Note: DNS propagation may take 5–30 minutes (up to 24 hours).

🌍 Step 3: Create Root Domain Alias Record (A → ALB)

Go to Route 53 → Hosted zones → vineethsworld.xyz

Click Create record

Configure the record as follows:

Field	Value
Record name	(leave empty)
Record type	A
Alias	ON
Route traffic to	Application Load Balancer
Region	us-east-1
Load balancer	your-alb.amazonaws.com

Click Create

✅ Result:

vineethsworld.xyz → ALB

🌐 Step 4: Create www Subdomain (CNAME)

Create another record:

Field	Value
Record name	www
Record type	CNAME
Value	your-alb.amazonaws.com

✅ Result:

www.vineethsworld.xyz → ALB

🔐 Step 5: Request SSL Certificate (ACM)

Go to AWS Certificate Manager (Region: us-east-1)

Click Request a certificate

Add the following domain names:

vineethsworld.xyz

www.vineethsworld.xyz

Select DNS validation

Click Request

🧾 Step 6: Validate Certificate Using DNS

In ACM, view the pending certificate

Click Create records in Route 53

Wait until the certificate status becomes:

Issued

🔒 Step 7: Add HTTPS Listener (443) to ALB

Navigate to EC2 → Load Balancers → myapp-alb → Listeners

Click Add listener

Configure:

Field	Value
Protocol	HTTPS
Port	443
Certificate	Select ACM certificate
Default action	Forward to target group

Save the listener

🔁 Step 8: Redirect HTTP to HTTPS

Edit the HTTP : 80 listener

Change the action to:

Redirect to HTTPS : 443

Save changes

🎯 Step 9: Create a Target Group

Go to EC2 → Target Groups → Create target group

Configure:

Setting	Value
Target type	Instances
Protocol	HTTP
Port	80
Health check path	/

Register EC2 instances

Attach the target group to the HTTPS listener

🩺 Step 10: Configure Health Checks

In Target Group → Health checks:

Field	Value
Path	/
Healthy threshold	2
Unhealthy threshold	2
📊 Step 11: Enable ALB Access Logs

Go to EC2 → Load Balancers → myapp-alb → Attributes

Enable:

Access logs: ON

S3 bucket: Select or create a bucket

Save changes

✅ Step 12: Verify Domain & HTTPS

Open the following URLs in a browser:

https://vineethsworld.xyz

https://www.vineethsworld.xyz

✔ Expected results:

HTTPS enabled 🔒

Website loads successfully

🌍 Step 13: Check DNS Propagation

Visit dnschecker.org

Enter: vineethsworld.xyz

Select A Record

🟢 Green status indicates global DNS propagation.

🧠 Final Architecture
vineethsworld.xyz
        ↓
     Route 53
        ↓
   ALB (HTTPS 443)
        ↓
   Target Group
        ↓
      EC2
