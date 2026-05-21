# PayBridge — System Architecture

> Companion to `paybrige-system-architecture.png`.
> Source posts:
> · Day 1 — [Why I'm Doing This Project, Publicly](https://mindthegrc.substack.com/p/why-im-doing-this-project-publicly)
> · Day 2 — [Designing PayBridge's Tech Stack — A Fictional Fintech Built for SOC 2](https://mindthegrc.substack.com/p/designing-paybridges-tech-stack-a)

---

## 1. What PayBridge is

PayBridge is a fictional fintech used as the working example for the *Mind the GRC* 14-day SOC 2 series. Its one-liner:

> PayBridge helps freelancers and small businesses receive international payments at live mid-market rates with zero forex markup.

The product is fictional; the architecture, data flows, and compliance pressures are all modelled on real cross-border-payment startups. That lets us walk through a complete SOC 2 implementation without ever touching real customer data.

---

## 2. Why architecture comes before compliance

Before you can secure anything, you need to know what you're securing. A SOC 2 auditor will ask:

- What systems exist?
- Where does data live?
- Who has access to what?
- How does data move between systems?

If those answers aren't clear, you're not ready for SOC 2 — full stop. The diagram and tables below are the answer key for PayBridge.

---

## 3. The stack, layer by layer

### Frontend
- **Next.js** for server-side rendering
- **React + TypeScript** for component-based UI with type safety
- **Tailwind CSS** for styling

*SOC 2 relevance:* this is the surface where users authenticate. Secure session handling and zero sensitive-data exposure in the browser are the obligations here.

### Backend API
- **Node.js + Express** for request handling and business logic
- **REST APIs** between frontend and backend
- **GraphQL** for internal data querying

*SOC 2 relevance:* all payment logic lives here. Access controls, input validation, and audit logging are enforced at this tier.

### Data tier
- **PostgreSQL (AWS RDS)** — primary store for transactions and user data; row-level security for multi-tenant isolation
- **Redis** — session and cache layer
- **MongoDB** — KYC documents and unstructured data

*SOC 2 relevance:* the data tier is the highest-value target. Encryption at rest, backup policies, and tightly scoped access are non-negotiable.

### Cloud platform — AWS
- **EC2** — compute
- **RDS** — managed PostgreSQL
- **S3** — document storage
- **CloudTrail** — audit logging
- **KMS** — encryption key management
- **VPC** — network isolation

*SOC 2 relevance:* the foundation everything else sits on. A misconfigured AWS account is the single biggest SOC 2 risk in this design.

### Security
- **OAuth 2.0 + JWT** — authentication
- **TLS 1.3** — data-in-transit encryption
- **AWS KMS** — key management
- **MFA** — required for all admin access

### Monitoring & observability
- **AWS CloudWatch** — infrastructure monitoring
- **Datadog** — application performance monitoring
- **PagerDuty** — on-call and incident alerting

### CI/CD
- **GitHub** — version control
- **GitHub Actions** — automated tests and deploys
- **Docker** — containerisation
- **Kubernetes** — orchestration

---

## 4. Data flow (matches the diagram)

```
User Browser  (TLS 1.3)
        |
        v
   Next.js Frontend
        |
        v
   Node.js + Express API
       /   |   \
      v    v    v
 Postgres Redis Mongo
        |
        v
 AWS Cloud Platform
   (EC2 / RDS / S3 / CloudTrail / KMS / VPC)
```

CI/CD pipelines deliver code into the backend tier; the security layer (OAuth, JWT, MFA, KMS) wraps frontend and backend; monitoring (CloudWatch / Datadog / PagerDuty) observes every tier.

---

## 5. SOC 2 control mapping

| Architecture layer | Primary SOC 2 control area |
|---|---|
| Frontend | Session management, MFA |
| Backend API | Access control, input validation, audit logging |
| Data tier | Encryption at rest, backup, access logs |
| AWS platform | Network security, audit logging, key management |
| CI/CD | Change management, code review, deploy gates |
| Monitoring | Incident detection, alerting, availability |

Mapped to the five Trust Service Criteria:

- **Security** — MFA, OAuth, KMS, VPC, GuardDuty
- **Availability** — CloudWatch, PagerDuty, multi-AZ RDS, K8s self-healing
- **Confidentiality** — TLS 1.3, KMS, S3 server-side encryption
- **Processing Integrity** — input validation, GitHub Actions test gates
- **Privacy** — Postgres row-level security, encrypted KYC store, retention policies

---

## 6. Intentional choices worth calling out

**Why AWS over GCP or Azure?**
AWS has the most mature compliance tooling — CloudTrail, Security Hub, GuardDuty — all built with SOC 2 readiness in mind.

**Why PostgreSQL?**
Structured financial data needs relational integrity. Postgres also supports row-level security, which is critical for multi-tenant fintech apps.

**Why GitHub Actions for CI/CD?**
SOC 2 requires demonstrable change management. Actions gives us automated test gates on every PR before code can reach production.

---

## 7. What's next in the series

The Day 2 post sets up the rest of the 14-day roadmap:

- Day 3 — SOC 2 simply explained
- Day 4 — Defining the scope
- Day 5 — Gap analysis
- Day 6 — Securing AWS
- Day 7 — Week 1 recap
- Day 8 — Access control
- Day 9 — Encryption
- Day 10 — Logging & monitoring
- Day 11 — Writing policies
- Day 12 — Vendor risk management
- Day 13 — Simulating an audit
- Day 14 — Final recap & all resources

Each subsequent post will add the corresponding artefacts to this repository (policies, control matrices, audit evidence templates, etc.).

---

*All diagrams and docs in this folder are free to use and adapt.*
