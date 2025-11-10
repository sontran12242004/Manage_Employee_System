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

**Enterprise HR Management System** là giải pháp quản lý nhân sự tích hợp được thiết kế cho doanh nghiệp vừa tại Việt Nam, hỗ trợ quản lý **100-500 nhân viên**. Hệ thống tự động hóa toàn bộ quy trình HR từ quản lý hồ sơ, chấm công, tính lương đến đánh giá hiệu suất. Đây là **dự án in-house** do team tự phát triển, tập trung vào **MVP với chi phí tối ưu dưới $100/tháng** trong giai đoạn đầu (100 nhân viên), sử dụng **AWS serverless architecture** với Lambda, API Gateway, DynamoDB để đảm bảo hiệu năng cao và chi phí thấp.

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

Hệ thống sử dụng **AWS Serverless Architecture** để tối ưu chi phí:

* **Compute:** AWS Lambda (pay-per-use, no idle costs).
* **API:** API Gateway REST API.
* **Database:** DynamoDB (on-demand billing).
* **Cache:** ElastiCache Redis (cache.t3.micro) - optional cho phase 2.
* **Authentication:** **AWS Cognito** (free tier <50K MAU).
* **Storage:** S3 cho documents, CloudFront CDN.
* **CI/CD:** **GitHub Actions** tự động deploy.
* **Monitoring:** **CloudWatch** (free tier).
* **Security:** Route 53, WAF (cost-optimized rules), IAM Roles.

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
* Chi phí chỉ **$45-70/tháng** cho 100 nhân viên (rẻ hơn 90% so với SAP/Workday).
* **In-house development** - không có chi phí outsourcing.

---

## 3. Kiến trúc giải pháp

Đây là sơ đồ kiến trúc đám mây của hệ thống:

![HR System Architecture](/images/2-Proposal/proposalaws.jpg)

### Dịch vụ AWS sử dụng

| Dịch vụ AWS | Chức năng chính |
| :--- | :--- |
| **AWS Lambda** | Backend API logic (Node.js 20.x) |
| **API Gateway** | REST API endpoints, request validation |
| **Amazon DynamoDB** | NoSQL database (on-demand billing) |
| **AWS Cognito** | Authentication, SSO (Google/Microsoft), JWT tokens |
| **Amazon S3** | Document storage (CV, contracts, payslips) |
| **CloudFront** | CDN cho static assets và S3 |
| **Route 53** | DNS management |
| **AWS WAF** (optional Phase 2) | API protection |
| **CloudWatch** | Logs, monitoring (free tier) |
| **Secrets Manager** | API keys, credentials |

### Thiết kế thành phần

#### Authentication Layer
* Cognito User Pools với JWT (RS256).
* Lambda authorizer cho API Gateway.
* MFA tùy chọn (SMS/TOTP) - Phase 2.

#### API Layer
* **AWS Lambda functions** (Node.js) deployed via GitHub Actions.
* API Gateway REST API với resource-based routing.
* Rate limiting (10 requests/second).
* CORS configured cho web/mobile.

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
* S3 Standard cho documents mới (<30 days).
* S3 Lifecycle → Glacier Deep Archive (>90 days).
* Presigned URLs cho secure upload/download.
* CloudFront distribution cho static web hosting.

#### Frontend
* **Next.js 14** (React 18) + TypeScript - Static export.
* Material-UI components.
* Hosted trên **CloudFront + S3** (no server cost).
* Mobile app: **React Native** (Expo) với AsyncStorage.

#### CI/CD Pipeline
* **GitHub Actions** workflow:
  * Build Lambda functions → ZIP packages
  * Deploy to Lambda via AWS CLI
  * Update API Gateway configurations
  * Deploy frontend to S3
* Automated Jest unit tests.

---

## 4. Triển khai kỹ thuật

### Giai đoạn 1: MVP Core (Tháng 1-2)
* **Tháng 1:**
  * AWS setup (Cognito, DynamoDB tables, S3, Lambda).
  * Authentication + Login UI.
  * Employee CRUD APIs + admin dashboard.
  
* **Tháng 2:**
  * Attendance check-in/out APIs với GPS.
  * Mobile app MVP (React Native).
  * Leave request workflow.
  * Basic reporting dashboard.

### Giai đoạn 2: Payroll & Automation (Tháng 3-4)
* **Tháng 3:**
  * Payroll calculation engine (Lambda).
  * Payslip generation (PDF via Lambda layer).
  * Approval workflows.
  
* **Tháng 4:**
  * Email notifications (SES).
  * Audit logging to DynamoDB.
  * Export reports (CSV).
  * Performance optimization.

### Giai đoạn 3: Advanced Features (Tháng 5-6)
* Performance review module.
* Training tracking.
* Advanced analytics dashboard.
* Security hardening.
* Load testing & optimization.
* User training & documentation.

### Tech Stack

| Thành phần | Công nghệ/Dịch vụ |
| :--- | :--- |
| **Backend** | Node.js 20.x, AWS Lambda, AWS SDK v3 |
| **Database** | DynamoDB (single-table design pattern) |
| **Frontend** | Next.js 14, React 18, TypeScript, Material-UI v5 |
| **Mobile** | React Native (Expo), AsyncStorage |
| **Infrastructure as Code** | AWS SAM / Serverless Framework |
| **CI/CD** | GitHub Actions |

---

## 5. Lộ trình & Mốc triển khai

| Tháng | Giai đoạn | Deliverable Chính |
| :--- | :--- | :--- |
| **1-2** | MVP Core | Auth, Employee management, Attendance mobile app |
| **3-4** | Payroll & Automation | Payroll engine, approval workflows, notifications |
| **5-6** | Advanced & Launch | Analytics, performance reviews, UAT, go-live |

---

## 6. Ước tính ngân sách

### Chi phí AWS hàng tháng (Phase 1: 100 nhân viên, ~5,000 API calls/ngày)

#### Kiến trúc Serverless - Chi phí tối ưu

| Dịch vụ | Cấu hình | Chi phí/tháng |
| :--- | :--- | ---: |
| **AWS Lambda** | 150K invocations, 512MB, 500ms avg | $0 |
| ↳ *Free tier: 1M requests + 400K GB-seconds/month* | (Trong free tier) | |
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
| ↳ *(First 5GB free)* | (Trong free tier) | |
| **Secrets Manager** | 2 secrets | $0.80 |
| **SES (email)** | 500 emails/month | $0.05 |
| **Cognito** | <50K MAU | $0 |
| ↳ *(Free tier)* | (Trong free tier) | |
| **Data Transfer OUT** | 5GB to internet | $0.45 |
| **Contingency (10%)** | Buffer | $0.75 |
| | | |
| **TỔNG AWS/THÁNG (100 users)** | | **~$8.30** |

#### Chi phí khi scale lên 200 users (Phase 2)

| Dịch vụ | Thay đổi | Chi phí/tháng |
| :--- | :--- | ---: |
| Lambda | 300K invocations (vẫn trong free tier) | $0 |
| API Gateway | 300K requests | $0.30 |
| DynamoDB | 10GB, 2M reads, 1M writes | $9.50 |
| S3 + CloudFront | 40GB storage, 20GB transfer | $2.50 |
| Route 53, Secrets, SES, Transfer | (tương tự) | $2.20 |
| **ElastiCache Redis** | cache.t3.micro (optional) | $12.50 |
| **AWS WAF** | Basic protection (optional) | $7.00 |
| Contingency | | $3.40 |
| | | |
| **TỔNG (200 users, với cache + WAF)** | | **~$37.40** |
| **TỔNG (200 users, không cache/WAF)** | | **~$17.90** |

#### Chi phí khi scale lên 500 users (Phase 3)

| Lambda + API Gateway | 750K invocations | $3.50 |
| DynamoDB | 25GB, 5M reads, 2.5M writes | $32.50 |
| S3 + CloudFront + Transfer | 100GB storage, 50GB CDN | $7.50 |
| ElastiCache Redis | cache.t3.small | $25.00 |
| AWS WAF | 2 rules | $8.00 |
| Route 53, Secrets, SES, misc | | $3.00 |
| Contingency | | $8.00 |
| | | |
| **TỔNG (500 users)** | | **~$87.50** |

### Tóm tắt chi phí Hosting theo giai đoạn

| Giai đoạn | Users | Chi phí/tháng | Chi phí/năm |
| :--- | :---: | ---: | ---: |
| **Phase 1 MVP** | 100 | **$8-12** | **~$100-150** |
| **Phase 2 Growth** | 200 | **$18-38** | **~$220-450** |
| **Phase 3 Scale** | 500 | **$88-95** | **~$1,050** |

### Chi phí phát triển (In-house team - NO outsourcing cost)

**Giả định:** Team nội bộ đã có lương cố định, chỉ tính chi phí AWS và tools

| Hạng mục | Chi phí |
| :--- | ---: |
| AWS hosting (6 tháng dev/staging @ $5/mo) | $30 |
| GitHub Pro (team of 5) | $0 |
| ↳ *(Có thể dùng free tier)* | |
| Domain name (.com) | $12/year |
| Third-party libraries (optional) | $0 |
| **TỔNG CHI PHÍ PHÁT TRIỂN** | **~$42** |

**Lưu ý:** Chi phí nhân sự KHÔNG tính vì đây là team in-house đã có lương cố định

### Chi phí vận hành hàng năm (sau go-live)

| Hạng mục | Chi phí/năm |
| :--- | ---: |
| AWS Hosting (Phase 1: 100 users) | $100-150 |
| Third-party services (SMS cho MFA - optional) | $100 |
| Domain renewal | $12 |
| **TỔNG VẬN HÀNH/NĂM (Phase 1)** | **~$212-262** |

### ROI Analysis (In-house project)

**Chi phí đầu tư:**
* Setup + Dev tools: ~$42
* AWS (6 tháng dev): ~$30
* **Total initial: ~$72**

**Chi phí vận hành năm đầu:**
* Phase 1 (6 tháng, 100 users): $60
* Phase 2 (6 tháng, 200 users): $150
* **Total Year 1: ~$210**

**Tổng chi phí Year 1: ~$282**

**Tiết kiệm so với alternatives:**
* SAP SuccessFactors: $8-15/user/month = $9,600-18,000/year
* BambooHR: $6-10/user/month = $7,200-12,000/year
* Excel thủ công: 1 FTE HR admin = $12,000/year

**Savings Year 1: $6,918 - $17,718**
**ROI Year 1: 2,454% - 6,281%** 🚀

---

## 7. Đánh giá rủi ro & Giảm thiểu

| Rủi ro | Ảnh hưởng | Xác suất | Giảm thiểu |
| :--- | :--- | :--- | :--- |
| DynamoDB costs spike | Trung bình | Thấp | On-demand billing, CloudWatch alarms at $30 threshold |
| Lambda cold starts | Thấp | Trung bình | Keep functions warm, optimize bundle size <1MB |
| API Gateway rate limits | Trung bình | Thấp | Default 10K req/s sufficient, implement caching |
| Vendor lock-in (AWS) | Trung bình | Cao | Use Serverless Framework for portability |
| Team learning curve | Thấp | Trung bình | Start with 1-2 Lambda functions, expand gradually |

### Best practices tối ưu chi phí

* **Lambda:** Bundle size <1MB, reuse connections, avoid cold starts.
* **DynamoDB:** Single-table design, use GSIs carefully, on-demand billing.
* **S3:** Lifecycle policies to Glacier, presigned URLs, CloudFront caching.
* **API Gateway:** Response caching (30-60s), throttling.
* **CloudWatch:** Log retention 7 days, filter unnecessary logs.

---

## 8. Kết quả kỳ vọng

### Cải tiến kỹ thuật
* **85%** HR processes tự động hóa.
* Real-time dashboard với data < 5 seconds old.
* **< 1s** API response time (P95) với Lambda.
* **70%** nhân viên sử dụng mobile app.
* Zero server maintenance.
* **Infinite scalability** với serverless.

### Giá trị kinh doanh
* HR team giảm **60%** workload thủ công.
* Employee satisfaction tăng **40%** (self-service).
* **100%** audit trail cho compliance.
* Payroll accuracy **99.5%**.
* **Cost savings $6,900-17,700/year** vs alternatives.
* Chi phí vận hành **chỉ $8-12/tháng** cho 100 users.

### Tầm nhìn dài hạn
* Scale lên 500 users với chi phí ~$88/tháng.
* Tích hợp AI/ML (AWS Bedrock) cho predictive analytics.
* Multi-branch operations.
* Potential SaaS product.

---

## 9. Kết luận

Hệ thống HR Management với **Serverless Architecture** cung cấp:

✅ **Chi phí cực thấp:** Chỉ $8-12/tháng cho 100 users Phase 1  
✅ **No upfront cost:** ~$72 setup, không có chi phí outsourcing  
✅ **ROI khủng:** Tiết kiệm $6,900-17,700/năm so với alternatives  
✅ **Scalable:** Pay-as-you-go, auto-scale to 500+ users  
✅ **Zero maintenance:** Serverless = no server management  
✅ **Fast development:** 6 tháng MVP → production  

Đây là giải pháp **lý tưởng cho startup/SME** với team in-house muốn xây dựng hệ thống HR hiện đại mà không cần đầu tư lớn.