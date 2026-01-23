23/01/2026
Task:9 Terraform State Migration to Scale Instance Using count Without Destroying Existing Instance Task Description Currently, an infrastructure instance is provisioned using Terraform as a single resource (without count). The requirement is to scale this resource to 5 instances using the count meta-argument, while ensuring that the existing instance remains intact and is not destroyed or recreated. To achieve this, Terraform state file migration is required so the existing instance is mapped to count.index = 0. Objective Modify the Terraform configuration to use count = 5 Preserve the existing instance as the first instance ([0]) Prevent Terraform from destroying and recreating the existing resource Perform state file migration using Terraform state commands
# Terraform State Migration to Scale Instance Using `count`

## 📌 Task 9

Scale an existing Terraform-managed instance (created without `count`) to multiple instances using `count = 5`, **without destroying or recreating** the existing instance.

The existing instance must be preserved and mapped to `count.index = 0` using Terraform state migration.

---

## 🎯 Objective

* Modify Terraform configuration to use `count = 5`
* Preserve the existing instance as `resource[0]`
* Prevent Terraform from destroying the current instance
* Perform Terraform **state file migration** safely

---

## 🧱 Prerequisites

* Terraform installed
* Existing infrastructure already created using Terraform
* Terraform state file available (`terraform.tfstate`)

---

## 🔹 Step 1: Existing Terraform Configuration (Before Scaling)

The instance was originally created **without `count`**.

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abcd1234"
  instance_type = "t2.micro"

  tags = {
    Name = "web-server"
  }
}
```

Terraform state contains:

```
aws_instance.web
```

---

## 🔹 Step 2: Update Terraform Configuration to Use `count`

Modify the resource to use the `count` meta-argument.

⚠️ **Do NOT run `terraform apply` yet**

```hcl
resource "aws_instance" "web" {
  count         = 5
  ami           = "ami-0abcd1234"
  instance_type = "t2.micro"

  tags = {
    Name = "web-server-${count.index}"
  }
}
```

---

## 🔹 Step 3: Migrate Terraform State

Terraform still thinks the existing resource is:

```
aws_instance.web
```

But the new configuration expects:

```
aws_instance.web[0]
```

To prevent destruction, migrate the state.

### ✅ Run the state migration command

```bash
terraform state mv aws_instance.web aws_instance.web[0]
```

### ✔ What this does

* Moves the existing instance in the state file
* Maps it to `count.index = 0`
* Prevents destroy and recreate
* No downtime

---

## 🔹 Step 4: Verify State Migration

```bash
terraform state list
```

Expected output:

```
aws_instance.web[0]
```

---

## 🔹 Step 5: Terraform Plan

```bash
terraform plan
```

Expected behavior:

* Existing instance remains untouched
* Terraform plans to create **4 new instances**
* No resources marked for destroy

---

## 🔹 Step 6: Apply the Changes

```bash
terraform apply
```

Terraform will:

* Keep existing instance as `aws_instance.web[0]`
* Create new instances:

  * `aws_instance.web[1]`
  * `aws_instance.web[2]`
  * `aws_instance.web[3]`
  * `aws_instance.web[4]`

---

## 🏗 Final Result

| Index  | Instance Status               |
| ------ | ----------------------------- |
| web[0] | Existing instance (preserved) |
| web[1] | New                           |
| web[2] | New                           |
| web[3] | New                           |
| web[4] | New                           |

---

## 🧠 Interview Explanation

**Q: How did you scale a Terraform resource using `count` without downtime?**

**Answer:**

> I used `terraform state mv` to migrate the existing resource in the Terraform state file to `count.index = 0`. This ensured Terraform did not destroy the existing instance while allowing me to scale the resource using `count`.

---

## 🔑 Key Commands Summary

```bash
terraform state mv aws_instance.web aws_instance.web[0]
terraform plan
terraform apply
```

---

✅ **Task 9 completed successfully**
