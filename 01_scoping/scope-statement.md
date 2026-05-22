# PayBridge SOC 2 Scope Statement

## Formal Scope Statement
"The scope of PayBridge's SOC 2
examination includes the PayBridge
payment processing platform and
supporting infrastructure used to
receive, process and store international
payment data for businesses.
This includes the PayBridge web
application, backend API services,
PostgreSQL database, and AWS cloud
infrastructure (ap-south-1 region).
The examination covers the Security,
Availability and Confidentiality
Trust Service Criteria."

## Systems In Scope

| System | In Scope | Reason |
|--------|----------|--------|
| AWS Infrastructure | Yes | Hosts everything |
| PostgreSQL Database | Yes | Stores customer data |
| Node.js Backend API | Yes | Processes payments |
| Next.js Frontend | Yes | Customer interface |
| Redis Cache | Yes | Stores session data |
| GitHub | Yes | Code affects production |
| Datadog | Yes | Has access to logs |
| Developer laptops | Yes | Production access |
| Marketing website | No | No customer data |
| Slack | No | No payment data |

## People In Scope
- Engineering team
- DevOps team
- Security team
- Customer support (limited)
- Contractors with system access

## Processes In Scope
- Payment processing workflow
- User onboarding and KYC
- Incident response
- Access provisioning
- Backup and recovery
- Vendor onboarding

## Physical Locations
- Remote-first startup
- AWS data centers (ap-south-1)

## Trust Service Criteria Selected
- Security (CC) — Mandatory
- Availability (A) — Payments platform
- Confidentiality (C) — Financial data

