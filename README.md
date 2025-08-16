Perfect 👍 Let’s make your **GitHub README.md** for this project in a clean, professional style.

Here’s the full Markdown content you can paste directly into your repo:

````markdown
# 🚀 Cloud Security with AWS IAM

This project demonstrates how to **use AWS IAM to control access to AWS resources**.  
We’ll launch EC2 instances and manage access for different users (like an intern) using IAM policies, groups, and users.

---

## 📌 Project Overview
- Launch **EC2 instances** (Production + Development).
- Use **tags** to separate environments.
- Create an **IAM Policy** that allows access only to development resources.
- Set up an **Account Alias** for friendly logins.
- Create **IAM Groups & Users**.
- Test IAM access with the new intern user.
- (Optional) Use the **IAM Policy Simulator**.
- Clean up all resources to avoid charges.

---

## 🛠️ Prerequisites
- An **AWS Account** (Free Tier is fine)
- Access to **AWS Management Console**

---

## 📝 Step-by-Step Guide

### **Step 1 – Launch EC2 Instances**
1. Go to **EC2 Console**.
2. **Switch region** to closest to you.
3. Launch **Production Instance**:
   - Name: `nextwork-prod-<yourname>`
   - Tag: `Env=production`
   - AMI: Free tier eligible (Amazon Linux 2)
   - Instance Type: `t2.micro`
   - Proceed without key pair (project only)
4. Launch **Development Instance**:
   - Name: `nextwork-dev-<yourname>`
   - Tag: `Env=development`
5. ✅ Verify both instances have correct tags.

---

### **Step 2 – Create IAM Policy**
1. Go to **IAM → Policies → Create Policy**.
2. Choose **JSON** and paste:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "ec2:*",
         "Resource": "*",
         "Condition": {
           "StringEquals": {
             "ec2:ResourceTag/Env": "development"
           }
         }
       },
       {
         "Effect": "Allow",
         "Action": "ec2:Describe*",
         "Resource": "*"
       },
       {
         "Effect": "Deny",
         "Action": [
           "ec2:DeleteTags",
           "ec2:CreateTags"
         ],
         "Resource": "*"
       }
     ]
   }
````

3. Name: `NextWorkDevEnvironmentPolicy`
4. Description: *IAM Policy for development environment*
5. ✅ Create policy.

---

### **Step 3 – Create AWS Account Alias**

1. Go to **IAM Dashboard**.
2. Under *Account Alias*, create:

   ```
   nextwork-alias-<yourname>
   ```
3. Your AWS sign-in URL:

   ```
   https://nextwork-alias-<yourname>.signin.aws.amazon.com/console/
   ```

---

### **Step 4 – Create IAM Groups and Users**

1. Create **User Group**:

   * Name: `nextwork-dev-group`
   * Attach policy: `NextWorkDevEnvironmentPolicy`
2. Create **User**:

   * Username: `nextwork-dev-<yourname>`
   * Enable: *AWS Console access*
   * Disable: *Password reset at next sign-in* (for project only)
   * Add to: `nextwork-dev-group`
3. ✅ Save user sign-in details.

---

### **Step 5 – Test Access**

1. Open sign-in URL:

   ```
   https://nextwork-alias-<yourname>.signin.aws.amazon.com/console/
   ```
2. Login with intern’s IAM user credentials.
3. Try stopping **Production instance** → ❌ Access Denied.
4. Try stopping **Development instance** → ✅ Success.

---

### **Step 6 – (Optional) IAM Policy Simulator**

1. Go to **IAM Policy Simulator**.
2. Select `nextwork-dev-<yourname>` user.
3. Simulate EC2 actions.
4. Verify only *development-tagged* resources are allowed.

---

### **Step 7 – Cleanup**

⚠️ Always clean up to avoid charges:

* Terminate EC2 instances.
* Delete IAM user.
* Delete IAM group.
* Delete IAM policy.
