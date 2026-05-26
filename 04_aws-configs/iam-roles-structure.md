# PayBridge IAM Roles Structure

## SOC 2 Control: CC6.1 — Least Privilege Access

## Roles

### 1. PayBridge-Admin-Role
**Who:** DevOps lead only
**Access:** Full AWS access
**MFA Required:** Yes — always
**Usage:** Emergency access only
**Review Frequency:** Monthly

### 2. PayBridge-Developer-Role
**Who:** All engineers
**Access:** Read production
         Full access to staging
         No delete in production
**MFA Required:** Yes
**Usage:** Daily development work
**Review Frequency:** Quarterly

### 3. PayBridge-ReadOnly-Role
**Who:** Finance, Operations, Support
**Access:** Read-only S3 and CloudWatch
**MFA Required:** Yes
**Usage:** Reporting and monitoring
**Review Frequency:** Quarterly

### 4. PayBridge-EC2-Instance-Role
**Who:** EC2 instances (automated)
**Access:** Specific S3 buckets
           CloudWatch Logs write
           KMS decrypt only
**MFA Required:** N/A (service role)
**Usage:** Application runtime
**Review Frequency:** Annually

### 5. PayBridge-RDS-Monitoring-Role
**Who:** RDS service (automated)
**Access:** CloudWatch metrics publish
**MFA Required:** N/A (service role)
**Usage:** Database monitoring
**Review Frequency:** Annually

## Access Review Process

Quarterly access review steps:
1. Export IAM user list from AWS Console
2. Cross-reference with current employee list from HR
3. Identify any terminated employees with active access
4. Identify any role changes requiring permission updates
5. Remove or modify access within 24 hours of identification
6. Document the review in access-review-[date].xlsx
7. Sign off by DevOps Lead and Security Owner

## Offboarding Checklist

When an employee leaves PayBridge:
- [ ] Disable IAM user account immediately
- [ ] Revoke all active sessions
- [ ] Remove from all IAM groups
- [ ] Remove MFA device
- [ ] Revoke GitHub access
- [ ] Remove Okta account
- [ ] Remove Datadog access
- [ ] Remove Slack access
- [ ] Remove any shared credentials
- [ ] Document completion date and approver
- [ ] Store completed checklist in HR folder
