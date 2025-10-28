---
title: "Proposal"
date: 2025-09-09
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# Enterprise HR Management System
## Comprehensive Cloud-Native HR Solution for Modern Enterprises

## 1. Executive Summary

The **Enterprise HR Management System** is an integrated human resources management solution designed for medium-sized enterprises in Vietnam, supporting the management of **100-500 employees**. The system automates the entire HR workflow, from employee record management, time and attendance, payroll calculation, to performance evaluation. The platform utilizes **AWS ECS on EC2** combined with **RDS PostgreSQL**, **S3 storage**, and a **CI/CD pipeline (GitHub Actions)** to ensure optimal cost **under $100/month**, high performance, comprehensive security, and detailed **RBAC** (Role-Based Access Control).

---

## 2. Problem Statement

### Current Challenges

* Vietnamese businesses rely on **Excel** or outdated HR software, leading to time consumption and errors.
* Manual processes (timekeeping, payroll) are **not integrated**.
* Lack of **automated approval workflows**.
* Difficulty managing **detailed access permissions**.
* Weak, **non-real-time reporting**.
* High costs for enterprise solutions like SAP or Workday.

### Proposed Solution

The system leverages an AWS ECS architecture optimized for cost efficiency:

* **Compute:** ECS Fargate on **EC2 (t3.medium Reserved Instance)** instead of Lambda to achieve **70% savings**.
* **Database:** RDS PostgreSQL **single-AZ** (instead of Multi-AZ) to minimize cost.
* **Authentication:** **AWS Cognito** for SSO + JWT.
* **Storage:** S3 for documents, basic CloudFront CDN.
* **CI/CD:** **GitHub Actions + CodeDeploy** for automated deployment.
* **Monitoring:** **CloudWatch** logs and alarms.
* **Security:** Route 53, WAF, IAM Roles, VPC with public/private subnets.

### Key Features

* **Single Sign-On** (Google, Microsoft 365).
* Detailed **RBAC** (Admin, Manager, Employee, Payroll Officer).
* **Check-in/out with GPS validation**.
* **Automated payroll** with flexible formula configuration.
* **Approval Workflows** (leave, salary adjustment).
* **Mobile App** (React Native) for attendance.
* **Real-time reporting dashboard**.
* Comprehensive audit logging.

### Benefits

* Saves **70%** of manual HR processing time.
* Reduces data entry errors by **90%**.
* Hosting cost is only **$80-95/month** (85% cheaper than SAP/Workday).
* Payback period is **10-14 months**.

---

## 3. Solution Architecture

Here is the cloud architecture diagram for the system:

![HR System Architecture](/images/2-Proposal/proposalaws.jpg)

### AWS Services Utilized

| AWS Service | Primary Function |
| :--- | :--- |
| **AWS Cognito** | Authentication, SSO (Google/Microsoft), JWT tokens |
| **Amazon RDS PostgreSQL** (db.t3.micro, single-AZ) | Employee, attendance, payroll data |
| **AWS ECS** | Docker containers for backend API + frontend |
| **EC2 t3.medium** (Reserved Instance) | Host ECS tasks (Cost-optimized Compute) |
| **Amazon S3** | Document storage (CV, contracts, payslips) |
| **CloudFront** | CDN for static assets, reduced S3 bandwidth |
| **Route 53** | DNS management |
| **AWS WAF** | API protection |
| **IAM Roles** | Fine-grained access control |
| **CloudWatch** | Logs, monitoring, alarms |
| **Secrets Manager** | API keys, DB credentials |
| **GitHub Actions** | CI/CD pipeline |
| **AWS CodeDeploy** | Automated deployment |

### Component Design

#### Authentication Layer
* Cognito User Pools with JWT (RS256).
* Custom authorizer middleware in the API.
* Optional MFA (SMS/TOTP).

#### API Layer
* Node.js Express.js service on ECS.
* RESTful endpoints for 15+ resources.
* Rate limiting, request validation.
* Cors configured for web/mobile.

#### Business Logic
* Employee management (CRUD, contracts, skills).
* Attendance tracking (check-in/out, GPS validation).
* Leave management (requests, approvals, balance).
* Payroll engine (salary calculation, tax, insurance).
* Performance reviews (KPI tracking).
* Workflow orchestration (approvals via email).

#### Data Layer
* RDS PostgreSQL: 12 tables (users, employees, departments, attendance_logs, payroll, leave_requests, approvals, etc.).
* Indexes on: `employee_id`, `department_id`, `date ranges`.
* Automated backups daily.

#### Storage Layer
* S3 buckets for documents (CV, contracts, payslips).
* S3 Lifecycle: transition to Glacier after 30 days.
* Signed URLs for secure download.
* CloudFront distribution for fast delivery.

#### Frontend
* **Next.js 14** (React 18) + TypeScript.
* Material-UI components.
* Hosted on **CloudFront + S3**.
* Mobile app: **React Native** (iOS/Android) with offline support.

#### CI/CD Pipeline
* **GitHub Actions** workflow: code push → build Docker image → push to ECR → deploy to ECS.
* Automated testing (**Jest** unit tests).
* Staging environment before production.

---

## 4. Technical Implementation

### Phase 1: Planning & Setup (Month 1)
* Requirements gathering.
* Database schema design (12 tables, ERD).
* API specification (OpenAPI/Swagger).
* AWS account setup, VPC, security groups.
* GitHub repository initialization.

### Phase 2: Infrastructure & Auth (Months 1-2)
* VPC with public/private subnets.
* RDS PostgreSQL provisioning (single-AZ).
* Cognito setup (email/phone login, SSO).
* S3 buckets for documents.
* IAM roles and policies.
* CI/CD pipeline (GitHub Actions + CodeDeploy).

### Phase 3: Core APIs & Mobile (Months 2-3)
* Employee management APIs.
* Attendance APIs with GPS validation.
* Check-in/out mobile app MVP.
* Leave management APIs.
* Docker containerization.

### Phase 4: Payroll & Workflows (Months 3-4)
* Payroll calculation engine.
* Payslip generation (PDF).
* Approval workflows (Lambda-free, using ECS scheduled tasks).
* Email notifications (SES).
* Audit logging.

### Phase 5: Analytics & Advanced (Months 4-5)
* Dashboard (attendance, payroll stats).
* Performance review module.
* Training tracking.
* Reporting (CSV exports).
* CloudWatch dashboards.

### Phase 6: Testing & Launch (Months 5-6)
* Unit & integration testing.
* UAT with 30 pilot users.
* Data migration from old system.
* Security audit (OWASP Top 10).
* Performance testing.
* Production deployment.
* End-user training.

### Tech Stack

| Component | Technology/Service |
| :--- | :--- |
| **Backend** | Node.js 20.x, Express.js, Prisma, Joi/Zod, JWT |
| **Database** | PostgreSQL 15, Automated backups (7 days) |
| **Frontend** | Next.js 14, React 18, TypeScript, Material-UI v5 |
| **Mobile** | React Native with Expo, Google Maps API |
| **Infrastructure as Code** | Terraform, Docker |
| **CI/CD** | GitHub Actions, AWS CodeDeploy |

---

## 5. Roadmap & Milestones

| Month | Phase | Key Deliverable |
| :--- | :--- | :--- |
| **1** | Planning + Infrastructure | AWS setup, database schema, API specs |
| **1-2** | Auth & Core Setup | Login with SSO, RBAC working |
| **2-3** | HR Core Modules | Employee management, attendance APIs, mobile app |
| **3-4** | Payroll & Workflows | Salary calculation, approval workflows |
| **4-5** | Analytics & Advanced | Dashboards, performance reviews, reporting |
| **5-6** | Testing & Launch | UAT, data migration, go-live |
| **6+** | Post-launch | Support, optimization, feature enhancements |

---

## 6. Budget Estimate

### Monthly AWS Cost (200 employees, 20,000 API calls/day)

| Service | Estimated Cost |
| :--- | :--- |
| EC2 t3.medium (Reserved, 1 year) | $20 |
| RDS PostgreSQL db.t3.micro | $25 |
| S3 storage (100GB, lifecycle) | $2.50 |
| S3 requests | $1 |
| CloudFront (50GB transfer) | $4 |
| Cognito (50K MAU free) | $0 |
| Route 53, CloudWatch, Secrets Manager, Data transfer | ~$7 |
| **Total AWS/month** | **$60-65** |

*Total Hosting (including GitHub Pro): **$64-69/month** (~$768-828/year)*

### Development Cost (One-Time)

| Category | Estimated Cost |
| :--- | :--- |
| Backend development (3 devs, 6 months) | $30,000 |
| Frontend development | $8,000 |
| Mobile app | $5,000 |
| DevOps / Infrastructure | $3,000 |
| QA & Testing | $4,000 |
| Project management | $3,000 |
| **Total Development** | **$53,000** |

### ROI Analysis

* Initial cost: $53,000 + $70 \* 6 = $53,420
* Annual savings: ~$29,000/year (2 FTE reduction + error reduction)
* **Payback period: ~22 months**
* NPV (3 years, 10% discount): $16,000-20,000

---

## 7. Risk Assessment & Mitigation

| Risk | Impact | Probability | Mitigation |
| :--- | :--- | :--- | :--- |
| Data breach | High | Medium | WAF, VPC, encryption, audit logs, MFA |
| RDS single-AZ downtime | High | Low | Multi-AZ failover after Phase 1, automated backup |
| Budget overrun | Medium | Low | AWS Cost Explorer alerts, reserved instances |
| Timeline delay | Medium | Medium | Agile + 20% buffer, MVP approach |
| User adoption | High | Medium | Training, change management, pilot program |

### Disaster Recovery Strategy
* RTO < 4 hours (restore from S3 backup)
* RPO < 1 hour (daily backups)
* Tested restore procedure monthly
* Post-launch: upgrade to Multi-AZ RDS

---

## 8. Expected Outcomes

### Technical Improvements
* **85%** of HR processes automated.
* Real-time dashboard.
* **< 2s** API response time.
* **70%** employee mobile app usage.
* Single source of truth.

### Business Value
* HR team workload reduced by **60%** (manual tasks).
* Employee satisfaction increased by **40%** (self-service).
* **100%** audit trail for compliance.
* Payroll accuracy **99.5%**.
* Cost savings **$29,000/year**.

### Long-Term Vision
* 1-2 years of data for **AI/ML** application.
* Platform scalability for new branches.
* Templatized HR modules.
* Competitive advantage through technology.