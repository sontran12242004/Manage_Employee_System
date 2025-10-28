---
title: "Bản đề xuất"
date: 2025-09-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
---



# Enterprise HR Management System
## Giải pháp quản lý nhân sự toàn diện cho doanh nghiệp hiện đại

## 1. Tóm tắt điều hành

**Enterprise HR Management System** là giải pháp quản lý nhân sự tích hợp được thiết kế cho doanh nghiệp vừa tại Việt Nam, hỗ trợ quản lý **100-500 nhân viên**. Hệ thống tự động hóa toàn bộ quy trình HR từ quản lý hồ sơ, chấm công, tính lương đến đánh giá hiệu suất. Nền tảng sử dụng **AWS ECS trên EC2** kết hợp **RDS PostgreSQL**, **S3 storage**, và **CI/CD pipeline (GitHub Actions)** để đảm bảo chi phí tối ưu **dưới $100/tháng** với hiệu năng cao, bảo mật đầy đủ và hệ thống **RBAC** chi tiết.

---

## 2. Tuyên bố vấn đề

### Vấn đề hiện tại

* Doanh nghiệp Việt Nam sử dụng **Excel** hay phần mềm HR cũ, gây tốn thời gian và sai sót.
* Quy trình thủ công (chấm công, tính lương) **không tích hợp**.
* Không có **workflow phê duyệt tự động**.
* Khó quản lý **phân quyền chi tiết**.
* Báo cáo yếu, **không real-time**.
* Chi phí cao cho giải pháp SAP, Workday.

### Giải pháp đề xuất

Hệ thống sử dụng AWS ECS với kiến trúc hiệu quả chi phí:

* **Compute:** ECS Fargate on **EC2 (t3.medium Reserved Instance)** thay vì Lambda để tiết kiệm 70%.
* **Database:** RDS PostgreSQL **single-AZ** (không Multi-AZ) để giảm chi phí.
* **Authentication:** **AWS Cognito** cho SSO + JWT.
* **Storage:** S3 cho documents, CloudFront CDN cơ bản.
* **CI/CD:** **GitHub Actions + CodeDeploy** tự động deploy.
* **Monitoring:** **CloudWatch** logs và alarms.
* **Security:** Route 53, WAF, IAM Roles, VPC với public/private subnets.

### Tính năng chính

* **Single Sign-On** (Google, Microsoft 365).
* **RBAC** chi tiết (Admin, Manager, Employee, Payroll Officer).
* **Check-in/out có GPS validation**.
* **Tính lương tự động** theo công thức linh hoạt.
* **Workflow phê duyệt** (leave, salary adjustment).
* **Mobile app** (React Native) cho attendance.
* **Dashboard báo cáo realtime**.
* Audit log đầy đủ.

### Lợi ích

* Tiết kiệm **70%** thời gian xử lý HR thủ công.
* Giảm **90%** sai sót nhập liệu.
* Chi phí chỉ **$80-95/tháng** (rẻ hơn 85% so với SAP/Workday).
* Thời gian hoàn vốn **10-14 tháng**.

---

## 3. Kiến trúc giải pháp

Đây là sơ đồ kiến trúc đám mây của hệ thống:

![HR System Architecture](/images/2-Proposal/proposalaws.jpg)

### Dịch vụ AWS sử dụng

| Dịch vụ AWS | Chức năng chính |
| :--- | :--- |
| **AWS Cognito** | Authentication, SSO (Google/Microsoft), JWT tokens |
| **Amazon RDS PostgreSQL** (db.t3.micro, single-AZ) | Employee, attendance, payroll data |
| **AWS ECS** | Docker containers cho backend API + frontend |
| **EC2 t3.medium** (Reserved Instance) | Host ECS tasks (Compute tối ưu chi phí) |
| **Amazon S3** | Document storage (CV, contracts, payslips) |
| **CloudFront** | CDN cho static assets, giảm băng thông S3 |
| **Route 53** | DNS management |
| **AWS WAF** | API protection |
| **IAM Roles** | Fine-grained access control |
| **CloudWatch** | Logs, monitoring, alarms |
| **Secrets Manager** | API keys, DB credentials |
| **GitHub Actions** | CI/CD pipeline |
| **AWS CodeDeploy** | Automated deployment |

### Thiết kế thành phần

#### Authentication Layer
* Cognito User Pools với JWT (RS256).
* Custom authorizer middleware trong API.
* MFA tùy chọn (SMS/TOTP).

#### API Layer
* Node.js Express.js service trên ECS.
* RESTful endpoints cho 15+ resources.
* Rate limiting, request validation.
* Cors configured cho web/mobile.

#### Business Logic
* Employee management (CRUD, contracts, skills).
* Attendance tracking (check-in/out, GPS validation).
* Leave management (requests, approvals, balance).
* Payroll engine (salary calculation, tax, insurance).
* Performance reviews (KPI tracking).
* Workflow orchestration (approvals via email).

#### Data Layer
* RDS PostgreSQL: 12 tables (users, employees, departments, attendance_logs, payroll, leave_requests, approvals, etc.).
* Indexes trên: `employee_id`, `department_id`, `date ranges`.
* Automated backups daily.

#### Storage Layer
* S3 buckets cho documents (CV, contracts, payslips).
* S3 Lifecycle: transition to Glacier after 30 days.
* Signed URLs cho secure download.
* CloudFront distribution cho fast delivery.

#### Frontend
* **Next.js 14** (React 18) + TypeScript.
* Material-UI components.
* Hosted trên **CloudFront + S3**.
* Mobile app: **React Native** (iOS/Android) với hỗ trợ offline.

#### CI/CD Pipeline
* **GitHub Actions** workflow: code push → build Docker image → push to ECR → deploy to ECS.
* Automated testing (**Jest** unit tests).
* Staging environment before production.

---

## 4. Triển khai kỹ thuật

### Giai đoạn 1: Planning & Setup (Tháng 1)
* Requirements gathering.
* Database schema design (12 tables, ERD).
* API specification (OpenAPI/Swagger).
* AWS account setup, VPC, security groups.
* GitHub repository initialization.

### Giai đoạn 2: Infrastructure & Auth (Tháng 1-2)
* VPC with public/private subnets.
* RDS PostgreSQL provisioning (single-AZ).
* Cognito setup (email/phone login, SSO).
* S3 buckets for documents.
* IAM roles and policies.
* CI/CD pipeline (GitHub Actions + CodeDeploy).

### Giai đoạn 3: Core APIs & Mobile (Tháng 2-3)
* Employee management APIs.
* Attendance APIs with GPS validation.
* Check-in/out mobile app MVP.
* Leave management APIs.
* Docker containerization.

### Giai đoạn 4: Payroll & Workflows (Tháng 3-4)
* Payroll calculation engine.
* Payslip generation (PDF).
* Approval workflows (Lambda-free, using ECS scheduled tasks).
* Email notifications (SES).
* Audit logging.

### Giai đoạn 5: Analytics & Advanced (Tháng 4-5)
* Dashboard (attendance, payroll stats).
* Performance review module.
* Training tracking.
* Reporting (CSV exports).
* CloudWatch dashboards.

### Giai đoạn 6: Testing & Launch (Tháng 5-6)
* Unit & integration testing.
* UAT with 30 pilot users.
* Data migration from old system.
* Security audit (OWASP Top 10).
* Performance testing.
* Production deployment.
* End-user training.

### Tech Stack

| Thành phần | Công nghệ/Dịch vụ |
| :--- | :--- |
| **Backend** | Node.js 20.x, Express.js, Prisma, Joi/Zod, JWT |
| **Database** | PostgreSQL 15, Automated backups (7 days) |
| **Frontend** | Next.js 14, React 18, TypeScript, Material-UI v5 |
| **Mobile** | React Native with Expo, Google Maps API |
| **Infrastructure as Code** | Terraform, Docker |
| **CI/CD** | GitHub Actions, AWS CodeDeploy |

---

## 5. Lộ trình & Mốc triển khai

| Tháng | Giai đoạn | Deliverable Chính |
| :--- | :--- | :--- |
| **1** | Planning + Infrastructure | AWS setup, database schema, API specs |
| **1-2** | Auth & Core Setup | Login with SSO, RBAC working |
| **2-3** | HR Core Modules | Employee management, attendance APIs, mobile app |
| **3-4** | Payroll & Workflows | Salary calculation, approval workflows |
| **4-5** | Analytics & Advanced | Dashboards, performance reviews, reporting |
| **5-6** | Testing & Launch | UAT, data migration, go-live |
| **6+** | Post-launch | Support, optimization, feature enhancements |

---

## 6. Ước tính ngân sách

### Chi phí AWS hàng tháng (200 nhân viên, 20,000 API calls/ngày)

| Dịch vụ | Chi phí ước tính |
| :--- | :--- |
| EC2 t3.medium (Reserved, 1 year) | $20 |
| RDS PostgreSQL db.t3.micro | $25 |
| S3 storage (100GB, lifecycle) | $2.50 |
| S3 requests | $1 |
| CloudFront (50GB transfer) | $4 |
| Cognito (50K MAU free) | $0 |
| Route 53, CloudWatch, Secrets Manager, Data transfer | ~$7 |
| **Tổng AWS/tháng** | **$60-65** |

*Tổng toàn bộ Hosting (bao gồm GitHub Pro): **$64-69/tháng** (~$768-828/năm)*

### Chi phí phát triển (one-time)

| Hạng mục | Chi phí ước tính |
| :--- | :--- |
| Backend development (3 devs, 6 tháng) | $30,000 |
| Frontend development | $8,000 |
| Mobile app | $5,000 |
| DevOps / Infrastructure | $3,000 |
| QA & Testing | $4,000 |
| Project management | $3,000 |
| **Tổng phát triển** | **$53,000** |

### ROI Analysis

* Initial cost: $53,000 + $70 \* 6 = $53,420
* Annual savings: ~$29,000/year (giảm 2 FTE + giảm lỗi)
* **Payback period: ~22 months**
* NPV (3 years, 10% discount): $16,000-20,000

---

## 7. Đánh giá rủi ro & Giảm thiểu

| Rủi ro | Ảnh hưởng | Xác suất | Giảm thiểu |
| :--- | :--- | :--- | :--- |
| Data breach | Cao | Trung bình | WAF, VPC, encryption, audit logs, MFA |
| RDS single-AZ downtime | Cao | Thấp | Multi-AZ failover sau phase 1, automated backup |
| Budget overrun | Trung bình | Thấp | AWS Cost Explorer alerts, reserved instances |
| Timeline delay | Trung bình | Trung bình | Agile + 20% buffer, MVP approach |
| User adoption | Cao | Trung bình | Training, change management, pilot program |

### Chiến lược DR
* RTO < 4 hours (restore from S3 backup)
* RPO < 1 hour (daily backups)
* Tested restore procedure monthly
* Post-launch: upgrade to Multi-AZ RDS

---

## 8. Kết quả kỳ vọng

### Cải tiến kỹ thuật
* **85%** HR processes tự động hóa.
* Real-time dashboard.
* **< 2s** API response time.
* **70%** nhân viên dùng mobile app.
* Single source of truth.

### Giá trị kinh doanh
* HR team giảm **60%** workload thủ công.
* Employee satisfaction tăng **40%** (self-service).
* **100%** audit trail cho compliance.
* Payroll accuracy **99.5%**.
* Cost savings **$29,000/year**.

### Tầm nhìn dài hạn
* 1-2 năm dữ liệu để áp dụng **AI/ML**.
* Platform mở rộng cho chi nhánh khác.
* Template cho các HR modules.
* Competitive advantage về tech.






