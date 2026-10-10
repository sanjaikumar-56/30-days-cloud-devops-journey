# 🚀 Day 17 — AWS IAM: Users, Groups, Roles & Policies

## 📌 Overview

Day 17 of my 30-Day Cloud & DevOps Learning Journey focuses on AWS Identity and Access Management (IAM).

IAM helps control who can access AWS resources and which actions they can perform.

Today, I studied IAM identities, groups, roles, policies, permission evaluation, least privilege, and AccessDenied troubleshooting.

## 🎯 Learning Objectives

- Understand AWS IAM fundamentals.
- Differentiate users, groups, roles, and policies.
- Understand IAM policy JSON.
- Learn Allow and explicit Deny.
- Differentiate authentication and authorization.
- Understand the principle of least privilege.
- Learn how EC2 applications access AWS services using IAM roles.
- Troubleshoot common IAM permission errors.
- Explore IAM-related AWS CLI commands.

---

# 🔐 1. What Is AWS IAM?

IAM stands for Identity and Access Management.

It is an AWS service used to manage identities and permissions for accessing AWS resources.

### Common use cases

- Managing user access.
- Assigning permissions to applications.
- Controlling access to S3 buckets.
- Allowing EC2 instances to access AWS services.
- Managing cross-account access.
- Implementing least privilege.

### IAM architecture

```text
AWS Account
    |
    ▼
   IAM
    |
    ├── Users
    |
    ├── Groups
    |
    ├── Roles
    |
    └── Policies
           |
           ▼
      AWS Resources
           |
           ├── EC2
           ├── S3
           ├── RDS
           └── Other Services
```

---

# 👤 2. IAM User

An IAM user is an identity within an AWS account.

It may represent a person or a workload using long-term credentials.

### Example

```text
AWS Account
    |
    └── IAM User
          |
          └── Developer
```

Permissions can be assigned through policies attached directly to the user or through group membership.

### Important security practices

- Avoid unnecessary long-term access keys.
- Enable MFA where appropriate.
- Use least privilege.
- Never expose credentials in GitHub repositories.
- Prefer temporary credentials when appropriate.

---

# 👥 3. IAM Group

An IAM group is a collection of IAM users.

Groups make it easier to manage common permissions.

### Example

```text
Developers Group
       |
       ├── Developer A
       ├── Developer B
       └── Developer C
```

A policy attached to the group can grant permissions to its users.

Important: IAM groups contain users, not IAM roles.

---

# 🎭 4. IAM Role

An IAM role is an identity that an authorized principal can assume to obtain temporary credentials.

### Common use cases

- EC2 applications accessing S3.
- Lambda functions accessing AWS services.
- Cross-account access.
- Federated workforce access.

### EC2 role architecture

```text
EC2 Instance
      |
      ▼
IAM Role
      |
      ▼
Temporary Credentials
      |
      ▼
IAM Permissions
      |
      ▼
S3 Bucket
```

Using an appropriately configured IAM role avoids placing long-term AWS access keys in application code.

---

# 📄 5. IAM Policy

An IAM policy is a document that defines permissions.

Policies specify which actions are allowed or denied on resources, potentially under specified conditions.

### Common policy types

| Policy Type | Purpose |
|---|---|
| Identity-based policy | Defines permissions for users, groups, or roles |
| Resource-based policy | Defines access permissions on supported resources |
| Permissions boundary | Limits the maximum permissions available to an identity |
| Service control policy | Sets permission guardrails for AWS Organizations accounts |
| Session policy | Restricts permissions for a particular role session in supported scenarios |

The effective permissions depend on the applicable authorization rules and policies.

---

# 📝 6. IAM Policy JSON Structure

Example policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadS3Objects",
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-company-reports/*"
    }
  ]
}
```

This is an example of allowing object reads from a specific bucket.

Replace `my-company-reports` with the actual bucket name before using it.

### Explanation

| Element | Purpose |
|---|---|
| Version | Policy language version |
| Statement | Contains permission statements |
| Sid | Optional statement identifier |
| Effect | Allow or Deny |
| Action | AWS API action |
| Resource | Resource to which the statement applies |
| Condition | Optional conditions for applying a statement |

The `Resource` value ending in `/*` applies to objects in the bucket, not the bucket itself.

---

# 🟢 7. Allow vs Explicit Deny

### Allow example

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-company-reports/*"
}
```

This allows matching object-read requests, subject to other applicable authorization controls.

### Explicit Deny example

```json
{
  "Effect": "Deny",
  "Action": "s3:DeleteObject",
  "Resource": "arn:aws:s3:::my-company-reports/*"
}
```

This explicitly denies deleting objects matching the specified resource.

### Important rule

An applicable explicit Deny overrides an Allow.

An Allow statement alone does not guarantee that an operation will succeed. Other applicable policies and authorization controls can restrict access.

---

# 🔑 8. Authentication vs Authorization

| Authentication | Authorization |
|---|---|
| Verifies identity | Determines permitted actions |
| Answers: Who are you? | Answers: What can you do? |
| Example: Sign-in | Example: Read an S3 object |

### Workflow

```text
Identity
   |
   ▼
Authentication
   |
   ▼
Authorization
   |
   ▼
Evaluate Applicable Permissions
   |
   ├── Allowed
   |
   └── Denied
```

A user can successfully authenticate but still receive an AccessDenied error when attempting an unauthorized action.

---

# 🛡️ 9. Principle of Least Privilege

Least privilege means granting only the permissions required for a task.

### Example

A developer who only needs to read application logs should not automatically receive permission to delete EC2 instances or modify IAM policies.

### Recommended approach

```text
Developer
    |
    ▼
Required Task
    |
    ▼
Minimum Necessary Permissions
    |
    ▼
Specific AWS Resources
```

Least privilege reduces the risk of accidental changes and limits the impact of compromised credentials.

---

# 🔄 10. IAM Role vs Hardcoded Access Keys

### Risky approach

```text
EC2 Application
       |
       ▼
Hardcoded AWS Access Keys
       |
       ▼
AWS Service
```

Hardcoded credentials can leak through source code, logs, configuration files, and repositories.

### Recommended approach

```text
EC2 Application
       |
       ▼
Attached IAM Role
       |
       ▼
Temporary Credentials
       |
       ▼
AWS Service
```

An appropriately configured IAM role provides temporary credentials to the application through AWS-supported credential mechanisms.

Never commit AWS credentials to GitHub.

---

# 🧪 11. Hands-on Lab — Explore IAM

## Step 1: Open the AWS Management Console

Navigate to the official AWS Console and search for IAM.

## Step 2: Explore Users

Review users, group memberships, and attached policies.

## Step 3: Explore Groups

Understand how groups organize users and help assign shared permissions.

## Step 4: Explore Roles

Review role names, trust relationships, and attached policies.

## Step 5: Explore Policies

Inspect AWS-managed and customer-managed policies.

## Step 6: Examine JSON

Identify the Version, Statement, Effect, Action, and Resource fields.

## Step 7: Review Security Recommendations

Explore available IAM security recommendations without modifying existing security settings.

This introductory lab can be completed without creating billable resources.

---

# 🪣 12. Practical Example — S3 Object Read Policy

File: `policies/s3-object-read-policy.json`

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadObjectsInLearningBucket",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::my-learning-bucket-unique-name/*"
    }
  ]
}
```

### Objective

Allow an authorized identity to read objects from a specific S3 bucket.

### Important notes

- Replace the example bucket name before use.
- This policy does not grant `s3:ListBucket`.
- This policy does not allow uploading or deleting objects.
- Other applicable authorization controls can still deny access.
- Do not use real credentials in policy files or documentation.

---

# 🚨 13. Troubleshooting — AccessDenied

### Scenario

An EC2 application attempts to read an S3 object but receives:

```text
AccessDenied
```

### Step 1: Identify the active identity

```bash
aws sts get-caller-identity
```

This identifies the caller associated with the active AWS credentials.

### Step 2: Check the IAM role

Verify that the expected role is attached to the EC2 instance and that the application is using the intended credentials.

### Step 3: Check the required action

For object reads, verify:

```text
s3:GetObject
```

For listing bucket contents, the application may also require:

```text
s3:ListBucket
```

### Step 4: Check resource ARNs

Object-level resource:

```text
arn:aws:s3:::bucket-name/*
```

Bucket-level resource:

```text
arn:aws:s3:::bucket-name
```

### Step 5: Check other restrictions

Review applicable:

- Explicit Deny statements.
- Permissions boundaries.
- Session policies.
- AWS Organizations service control policies.
- S3 bucket policies.
- VPC endpoint policies, if applicable.
- KMS permissions for objects encrypted with AWS KMS.

### Step 6: Retest

After making an authorized and narrowly scoped permission change, retry the operation.

### Troubleshooting flow

```text
AccessDenied
     |
     ▼
Identify Caller
     |
     ▼
Check IAM Role or User
     |
     ▼
Check Required Action
     |
     ▼
Verify Resource ARN
     |
     ▼
Review Explicit Deny
     |
     ▼
Check Other Applicable Policies
     |
     ▼
Retest
```

Avoid granting AdministratorAccess simply to bypass an AccessDenied error.

---

# ⌨️ 14. AWS CLI Commands

### Check AWS CLI version

```bash
aws --version
```

### Identify the current caller

```bash
aws sts get-caller-identity
```

### List IAM users

```bash
aws iam list-users
```

### List IAM roles

```bash
aws iam list-roles
```

### List customer-managed policies

```bash
aws iam list-policies --scope Local
```

These commands require valid AWS credentials and the appropriate permissions.

If a command returns AccessDenied, verify the current identity and its authorized permissions.

---

# 🎯 15. Interview Questions

### Q1. What is AWS IAM?

IAM manages identities and permissions for accessing AWS resources.

### Q2. What is the difference between a user and a role?

A user is an identity within an AWS account. A role is an identity that an authorized principal can assume to obtain temporary credentials.

### Q3. What is an IAM policy?

An IAM policy defines permissions for actions and resources, optionally subject to conditions.

### Q4. What is least privilege?

It means granting only the minimum permissions required to perform a task.

### Q5. What is authentication vs authorization?

Authentication verifies identity, while authorization determines which actions the identity can perform.

### Q6. What happens when an explicit Deny applies?

An applicable explicit Deny overrides an Allow.

### Q7. Why use IAM roles on EC2?

IAM roles provide temporary credentials to applications and avoid the need to store long-term access keys in application code.

### Q8. How would you troubleshoot an S3 AccessDenied error?

I would identify the active caller, verify the attached IAM role and policies, check the required action and resource ARN, and investigate explicit Deny statements and other applicable authorization restrictions.

### Q9. Can an IAM group contain roles?

No. IAM groups contain IAM users, not IAM roles.

### Q10. Does an Allow policy guarantee access?

No. Other applicable authorization controls may restrict access, and an explicit Deny overrides an Allow.

---

# 📸 16. Screenshots

Recommended screenshots:

```text
screenshots/
├── 01-iam-dashboard.png
├── 02-iam-users.png
├── 03-iam-roles.png
├── 04-iam-policy-json.png
└── 05-iam-troubleshooting.png
```

Before publishing, remove sensitive account details and ensure no credentials or secrets appear in screenshots.

---

# 💡 17. Key Takeaways

- IAM manages identities and permissions.
- Users represent IAM identities.
- Groups organize IAM users.
- Roles provide assumable permissions and temporary credentials.
- Policies define permissions.
- Explicit Deny overrides an applicable Allow.
- Authentication and authorization are different.
- Least privilege is essential for cloud security.
- IAM roles are preferable to hardcoded long-term credentials for supported workloads.
- AccessDenied troubleshooting requires checking the identity, actions, resources, and applicable authorization controls.

---

# 🚀 Day 17 Completed

Today I strengthened my understanding of AWS IAM, permission policies, temporary credentials, least privilege, and AccessDenied troubleshooting.

Next: Day 18 — Amazon EC2: Instances, AMIs, Instance Types, Key Pairs, Security Groups, SSH, and Troubleshooting.
