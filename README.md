# aws-devops-labs
My AWS DevOps hands-on labs documentation
# AWS Lab 01 — Introduction to AWS IAM

**Series:** AWS Cloud Practitioner / DevOps Foundations Labs
**Difficulty:** Beginner | **Duration:** ~40 mins | **Region:** us-east-1 | **Service:** IAM

---

## Overview

This lab covers AWS Identity and Access Management (IAM) — the service that controls who can do what inside an AWS account. I explored users, groups, managed policies, inline policies, and tested real permission boundaries.

---

## What I Did

- Explored pre-created IAM users and groups
- Inspected managed and inline IAM policies
- Assigned users to groups based on business roles
- Signed in as each user and verified their permission limits

---

## Business Scenario — Role-Based Access

| User   | Group      | Policy                          | Permissions                        |
|--------|------------|---------------------------------|------------------------------------|
| user-1 | S3-Support  | AmazonS3ReadOnlyAccess (Managed)| List and view S3 objects only      |
| user-2 | EC2-Support | AmazonEC2ReadOnlyAccess (Managed)| View EC2 resources — no changes   |
| user-3 | EC2-Admin   | Custom EC2 Policy (Inline)      | View, Start, and Stop EC2 instances|

---

## IAM Policy Structure
```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "*"
}
```

Every policy has three parts: **Effect** (Allow/Deny), **Action** (what API call), and **Resource** (what it applies to).

---

## Key Concepts Learned

**1. Principle of Least Privilege**
IAM users start with zero permissions. Every permission must be explicitly granted — never give more access than the job requires.

**2. Managed vs Inline Policies**
Managed policies are reusable and can be attached to multiple groups. Inline policies are tied to a single group or user for specific one-off needs.

**3. Group-Based Access Management**
Permissions go on groups, not individual users. Users inherit access by joining a group — making role changes simple and scalable.

**4. Testing Permissions**
IAM must be tested, not assumed. Verifying that each user can only do what they're allowed to is a required step — misconfigured IAM is one of the top causes of cloud security breaches.

---

## DevOps Relevance

| Lab Concept         | Real-World Application                              |
|---------------------|-----------------------------------------------------|
| Users & Groups      | Staff onboarding/offboarding, team roles            |
| Least Privilege     | Zero-trust security, SOC2/ISO27001 compliance       |
| Managed Policies    | Standardised role templates across engineering teams|
| Inline Policies     | Specific access for CI/CD pipeline roles            |
| Permission Testing  | Security audits, IAM access reviews                 |

---

## What's Next

Next lab covers **IAM Roles** — how AWS services like EC2 and Lambda access other resources securely without hardcoded credentials. This is how real production systems are built.

---

## References

- [AWS IAM User Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html)
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [IAM Policies and Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
