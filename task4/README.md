
# 🛡️ Task-4: AWS WAF with Application Load Balancer

## 1️⃣ Create AWS WAF Web ACL

1. Go to **AWS Console → WAF & Shield**
2. Click **Create Web ACL**
3. Configure:
   - **Name:** `web-acl-alb`
   - **Resource type:** Regional
   - **Region:** Same as your ALB
   - **Associated AWS resource:** Select your **ALB (web-alb)**
4. Click **Next**

---

## 2️⃣ Create IP Set (Your Laptop IP)

You must block **your public IP**.

1. Go to **WAF → IP sets**
2. Click **Create IP set**
3. Configure:
   - **Name:** `blocked-ip`
   - **Region:** Same as ALB
   - **IP version:** IPv4
4. Find your public IP from:  
   https://whatismyipaddress.com
5. Enter your IP in CIDR format: 203.0.113.25/32
6. Click **Create**

---

## 3️⃣ Add Block Rule in Web ACL

1. Go to **WAF → Web ACLs → `web-acl-alb`**
2. Click **Add rule**
3. Configure:
- **Rule type:** IP set
- **Rule name:** `block-my-laptop`
- **IP set:** `blocked-ip`
- **Action:** Block
- **Priority:** 1
4. Click **Save**

Default action should be **Allow**.

---

## 4️⃣ Attach WAF to ALB

If not already attached:

1. Open **Web ACL → `web-acl-alb`**
2. Go to **Associated AWS resources**
3. Click **Add**
4. Select **`web-alb`**
5. Click **Save**

WAF is now protecting the ALB.

---

## 5️⃣ Test Before & After WAF

### A. Before Block Rule

1. Go to **EC2 → Load Balancers → `web-alb`**
2. Copy the DNS name: web-alb-123456.us-east-1.elb.amazonaws.com
3. Open it in your browser  
You should see:
- Auto Scaling Instance - ip-10-0-x-x

✅ Access allowed

---

### B. After Block Rule Added

1. Wait 1–2 minutes
2. Open the ALB URL again

You should see:
- 403 Forbidden
