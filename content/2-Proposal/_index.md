---
title: "Proposal"
date: 2025-09-09
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# Enterprise HR Management System
## Comprehensive HR Management Solution for Modern Enterprises

## 1. Executive Summary

**Enterprise HR Management System** is an integrated HR management solution designed for mid-sized enterprises in Vietnam, supporting **100-500 employees**. The system automates the entire HR workflow from profile management, attendance tracking, payroll calculation to performance evaluation. This is an **in-house project** developed by the team, focusing on **MVP with optimized costs under $100/month** in the initial phase (100 employees), utilizing **AWS serverless architecture** with Lambda, API Gateway, DynamoDB to ensure high performance and low costs.

---

## 2. Problem Statement

### Current Issues

* Vietnamese enterprises use **Excel** or legacy HR software, causing time waste and errors.
* Manual processes (attendance, payroll) are **not integrated**.
* No **automated approval workflows**.
* Difficult to manage **detailed permissions**.
* Weak reporting, **not real-time**.
* High costs for SAP, Workday solutions.

### Proposed Solution

The system uses **AWS Serverless Architecture** to optimize costs:

* **Compute:** AWS Lambda (pay-per-use, no idle costs).
* **API:** API Gateway REST API.
* **Database:** DynamoDB (on-demand billing).
* **Cache:** ElastiCache Redis (cache.t3.micro) - optional for phase 2.
* **Authentication:** **AWS Cognito** (free tier <50K MAU).
* **Storage:** S3 for documents, CloudFront CDN.
* **CI/CD:** **GitHub Actions** for automated deployment.
* **Monitoring:** **CloudWatch** (free tier).
* **Security:** Route 53, WAF (cost-optimized rules), IAM Roles.

### Key Features

* **Single Sign-On** (Google, Microsoft 365).
* **Detailed RBAC** (Admin, Manager, Employee, Payroll Officer).
* **Check-in/out with GPS validation**.
* **Automated payroll calculation** with flexible formulas.
* **Approval workflows** (leave, salary adjustment).
* **Mobile app** (React Native) for attendance.
* **Real-time reporting dashboard**.
* Comprehensive audit logs.

### Benefits

* Save **70%** of manual HR processing time.
* Reduce **90%** of data entry errors.
* Cost only **$45-70/month** for 100 employees (90% cheaper than SAP/Workday).
* **In-house development** - no outsourcing costs.

---

## 3. Solution Architecture

Here is the cloud architecture diagram of the system:

![HR System Architecture](/images/2-Proposal/proposalaws.jpg)

### AWS Services Used

| AWS Service | Primary Function |
| :--- | :--- |
| **AWS Lambda** | Backend API logic (Node.js 20.x) |
| **API Gateway** | REST API endpoints, request validation |
| **Amazon DynamoDB** | NoSQL database (on-demand billing) |
| **AWS Cognito** | Authentication, SSO (Google/Microsoft), JWT tokens |
| **Amazon S3** | Document storage (CV, contracts, payslips) |
| **CloudFront** | CDN for static assets and S3 |
| **Route 53** | DNS management |
| **AWS WAF** (optional Phase 2) | API protection |
| **CloudWatch** | Logs, monitoring (free tier) |
| **Secrets Manager** | API keys, credentials |

### Component Design

#### Authentication Layer
* Cognito User Pools with JWT (RS256).
* Lambda authorizer for API Gateway.
* Optional MFA (SMS/TOTP) - Phase 2.

#### API Layer
* **AWS Lambda functions** (Node.js) deployed via GitHub Actions.
* API Gateway REST API with resource-based routing.
* Rate limiting (10 requests/second).
* CORS configured for web/mobile.

#### Business Logic (Lambda Functions)
* Employee management (CRUD, contracts, skills).
* Attendance tracking (check-in/out, GPS validation).
* Leave management (requests, approvals, balance).
* Payroll engine (salary calculation, tax, insurance).
* Performance reviews (KPI tracking).
* Email notifications (SES free tier).

#### Data Layer - DynamoDB Tables
* **Users** - GSI on email
* **Employees** - GSI on department_id
* **Departments**
* **AttendanceLogs** - GSI on employee_id + date
* **LeaveRequests** - GSI on employee_id + status
* **PayrollRecords** - GSI on employee_id + month
* **Approvals** - GSI on approver_id + status

#### Storage Layer
* S3 Standard for new documents (<30 days).
* S3 Lifecycle → Glacier Deep Archive (>90 days).
* Presigned URLs for secure upload/download.
* CloudFront distribution for static web hosting.

#### Frontend
* **Next.js 14** (React 18) + TypeScript - Static export.
* Material-UI components.
* Hosted on **CloudFront + S3** (no server cost).
* Mobile app: **React Native** (Expo) with AsyncStorage.

#### CI/CD Pipeline
* **GitHub Actions** workflow:
  * Build Lambda functions → ZIP packages
  * Deploy to Lambda via AWS CLI
  * Update API Gateway configurations
  * Deploy frontend to S3
* Automated Jest unit tests.

---

## 4. Technical Implementation

### Phase 1: MVP Core (Month 1-2)
* **Month 1:**
  * AWS setup (Cognito, DynamoDB tables, S3, Lambda).
  * Authentication + Login UI.
  * Employee CRUD APIs + admin dashboard.
  
* **Month 2:**
  * Attendance check-in/out APIs with GPS.
  * Mobile app MVP (React Native).
  * Leave request workflow.
  * Basic reporting dashboard.

### Phase 2: Payroll & Automation (Month 3-4)
* **Month 3:**
  * Payroll calculation engine (Lambda).
  * Payslip generation (PDF via Lambda layer).
  * Approval workflows.
  
* **Month 4:**
  * Email notifications (SES).
  * Audit logging to DynamoDB.
  * Export reports (CSV).
  * Performance optimization.

### Phase 3: Advanced Features (Month 5-6)
* Performance review module.
* Training tracking.
* Advanced analytics dashboard.
* Security hardening.
* Load testing & optimization.
* User training & documentation.

### Tech Stack

| Component | Technology/Service |
| :--- | :--- |
| **Backend** | Node.js 20.x, AWS Lambda, AWS SDK v3 |
| **Database** | DynamoDB (single-table design pattern) |
| **Frontend** | Next.js 14, React 18, TypeScript, Material-UI v5 |
| **Mobile** | React Native (Expo), AsyncStorage |
| **Infrastructure as Code** | AWS SAM / Serverless Framework |
| **CI/CD** | GitHub Actions |

---

## 5. Roadmap & Milestones

| Month | Phase | Key Deliverables |
| :--- | :--- | :--- |
| **1-2** | MVP Core | Auth, Employee management, Attendance mobile app |
| **3-4** | Payroll & Automation | Payroll engine, approval workflows, notifications |
| **5-6** | Advanced & Launch | Analytics, performance reviews, UAT, go-live |

---

## 6. Budget Estimation

### Monthly AWS Costs (Phase 1: 100 employees, ~5,000 API calls/day)

#### Serverless Architecture - Cost Optimized

| Service | Configuration | Cost/Month |
| :--- | :--- | ---: |
| **AWS Lambda** | 150K invocations, 512MB, 500ms avg | $0 |
| ↳ *Free tier: 1M requests + 400K GB-seconds/month* | (Within free tier) | |
| **API Gateway** | 150K REST API requests/month | $0.15 |
| ↳ *$3.50 per million after first 1M (free tier year 1)* | | |
| **DynamoDB** | On-demand, 5GB storage, 1M reads, 500K writes | $3.50 |
| ↳ *Storage: $1.25/GB ($6.25) + Reads: $0.25/M + Writes: $1.25/M* | | |
| **S3 Storage** | 20GB documents (100 users) | $0.46 |
| **S3 Requests** | 20K PUT, 100K GET/month | $0.14 |
| **S3 Glacier (archive)** | 10GB old documents | $0.10 |
| **CloudFront** | 10GB transfer, 200K requests | $1.00 |
| **Route 53** | 1 hosted zone + 1M queries | $0.90 |
| **CloudWatch Logs** | 2GB logs/month | $0 |
| ↳ *(First 5GB free)* | (Within free tier) | |
| **Secrets Manager** | 2 secrets | $0.80 |
| **SES (email)** | 500 emails/month | $0.05 |
| **Cognito** | <50K MAU | $0 |
| ↳ *(Free tier)* | (Within free tier) | |
| **Data Transfer OUT** | 5GB to internet | $0.45 |
| **Contingency (10%)** | Buffer | $0.75 |
| | | |
| **TOTAL AWS/MONTH (100 users)** | | **~$8.30** |

#### Costs When Scaling to 200 Users (Phase 2)

| Service | Changes | Cost/Month |
| :--- | :--- | ---: |
| Lambda | 300K invocations (still in free tier) | $0 |
| API Gateway | 300K requests | $0.30 |
| DynamoDB | 10GB, 2M reads, 1M writes | $9.50 |
| S3 + CloudFront | 40GB storage, 20GB transfer | $2.50 |
| Route 53, Secrets, SES, Transfer | (similar) | $2.20 |
| **ElastiCache Redis** | cache.t3.micro (optional) | $12.50 |
| **AWS WAF** | Basic protection (optional) | $7.00 |
| Contingency | | $3.40 |
| | | |
| **TOTAL (200 users, with cache + WAF)** | | **~$37.40** |
| **TOTAL (200 users, without cache/WAF)** | | **~$17.90** |

#### Costs When Scaling to 500 Users (Phase 3)

| Lambda + API Gateway | 750K invocations | $3.50 |
| DynamoDB | 25GB, 5M reads, 2.5M writes | $32.50 |
| S3 + CloudFront + Transfer | 100GB storage, 50GB CDN | $7.50 |
| ElastiCache Redis | cache.t3.small | $25.00 |
| AWS WAF | 2 rules | $8.00 |
| Route 53, Secrets, SES, misc | | $3.00 |
| Contingency | | $8.00 |
| | | |
| **TOTAL (500 users)** | | **~$87.50** |

### Hosting Cost Summary by Phase

| Phase | Users | Cost/Month | Cost/Year |
| :--- | :---: | ---: | ---: |
| **Phase 1 MVP** | 100 | **$8-12** | **~$100-150** |
| **Phase 2 Growth** | 200 | **$18-38** | **~$220-450** |
| **Phase 3 Scale** | 500 | **$88-95** | **~$1,050** |

### Development Costs (In-house team - NO outsourcing cost)

**Assumption:** In-house team already has fixed salaries, only AWS and tools costs counted

| Item | Cost |
| :--- | ---: |
| AWS hosting (6 months dev/staging @ $5/mo) | $30 |
| GitHub Pro (team of 5) | $0 |
| ↳ *(Can use free tier)* | |
| Domain name (.com) | $12/year |
| Third-party libraries (optional) | $0 |
| **TOTAL DEVELOPMENT COST** | **~$42** |

**Note:** Personnel costs NOT included as this is an in-house team with fixed salaries

### Annual Operating Costs (post go-live)

| Item | Cost/Year |
| :--- | ---: |
| AWS Hosting (Phase 1: 100 users) | $100-150 |
| Third-party services (SMS for MFA - optional) | $100 |
| Domain renewal | $12 |
| **TOTAL OPERATING/YEAR (Phase 1)** | **~$212-262** |

### ROI Analysis (In-house project)

**Initial Investment:**
* Setup + Dev tools: ~$42
* AWS (6 months dev): ~$30
* **Total initial: ~$72**

**First Year Operating Costs:**
* Phase 1 (6 months, 100 users): $60
* Phase 2 (6 months, 200 users): $150
* **Total Year 1: ~$210**

**Total Year 1 Cost: ~$282**

**Savings vs Alternatives:**
* SAP SuccessFactors: $8-15/user/month = $9,600-18,000/year
* BambooHR: $6-10/user/month = $7,200-12,000/year
* Manual Excel: 1 FTE HR admin = $12,000/year

**Year 1 Savings: $6,918 - $17,718**
**Year 1 ROI: 2,454% - 6,281%** 🚀

---

## 7. Risk Assessment & Mitigation

| Risk | Impact | Probability | Mitigation |
| :--- | :--- | :--- | :--- |
| DynamoDB costs spike | Medium | Low | On-demand billing, CloudWatch alarms at $30 threshold |
| Lambda cold starts | Low | Medium | Keep functions warm, optimize bundle size <1MB |
| API Gateway rate limits | Medium | Low | Default 10K req/s sufficient, implement caching |
| Vendor lock-in (AWS) | Medium | High | Use Serverless Framework for portability |
| Team learning curve | Low | Medium | Start with 1-2 Lambda functions, expand gradually |

### Cost Optimization Best Practices

* **Lambda:** Bundle size <1MB, reuse connections, avoid cold starts.
* **DynamoDB:** Single-table design, use GSIs carefully, on-demand billing.
* **S3:** Lifecycle policies to Glacier, presigned URLs, CloudFront caching.
* **API Gateway:** Response caching (30-60s), throttling.
* **CloudWatch:** Log retention 7 days, filter unnecessary logs.

---

## 8. Expected Outcomes

### Technical Improvements
* **85%** HR processes automated.
* Real-time dashboard with data < 5 seconds old.
* **< 1s** API response time (P95) with Lambda.
* **70%** employees use mobile app.
* Zero server maintenance.
* **Infinite scalability** with serverless.

### Business Value
* HR team reduces **60%** manual workload.
* Employee satisfaction increases **40%** (self-service).
* **100%** audit trail for compliance.
* Payroll accuracy **99.5%**.
* **Cost savings $6,900-17,700/year** vs alternatives.
* Operating cost **only $8-12/month** for 100 users.

### Long-term Vision
* Scale to 500 users at ~$88/month cost.
* Integrate AI/ML (AWS Bedrock) for predictive analytics.
* Multi-branch operations.
* Potential SaaS product.

---

## 9. Conclusion

HR Management System with **Serverless Architecture** provides:

✅ **Ultra-low cost:** Only $8-12/month for 100 users Phase 1  
✅ **No upfront cost:** ~$72 setup, no outsourcing costs  
✅ **Massive ROI:** Save $6,900-17,700/year vs alternatives  
✅ **Scalable:** Pay-as-you-go, auto-scale to 500+ users  
✅ **Zero maintenance:** Serverless = no server management  
✅ **Fast development:** 6 months MVP → production  

This is an **ideal solution for startups/SMEs** with in-house teams wanting to build a modern HR system without large investments.