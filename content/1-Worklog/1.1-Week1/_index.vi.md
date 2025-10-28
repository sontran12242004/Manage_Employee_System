---
title: "Worklog Tuần 1"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

## Mục tiêu tuần 1

- Kết nối, làm quen với các thành viên trong First Cloud Journey
- Hiểu dịch vụ AWS cơ bản, cách sử dụng Console & CLI
- Tìm hiểu cách tối ưu hóa chi phí
- Tìm hiểu và cấu hình IAM
- Tạo Group và User, đồng thời kết nối từ máy tính lên đám mây
- Hiểu cấu trúc các lớp trên đám mây

## Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|-----------------|
| **Thứ 2** | • Làm quen với các thành viên FCJ<br>• Đọc và nắm vững các nội quy, quy định tại đơn vị thực tập | 09/09/2025 | 09/09/2025 | - |
| **Thứ 3** | • Tìm hiểu AWS và các loại dịch vụ:<br>&nbsp;&nbsp;◦ Compute<br>&nbsp;&nbsp;◦ Storage<br>&nbsp;&nbsp;◦ Networking<br>&nbsp;&nbsp;◦ Database<br>&nbsp;&nbsp;◦ Security & Identity | 12/08/2025 | 12/08/2025 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| **Thứ 4** | • Tạo AWS Free Tier account<br>• Tìm hiểu AWS Console & AWS CLI<br>• **Thực hành:**<br>&nbsp;&nbsp;◦ Tạo AWS account<br>&nbsp;&nbsp;◦ Cài đặt AWS CLI & cấu hình<br>&nbsp;&nbsp;◦ Sử dụng AWS CLI cơ bản | 13/08/2025 | 13/08/2025 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| **Thứ 5** | • Tìm hiểu EC2 cơ bản:<br>&nbsp;&nbsp;◦ Instance types<br>&nbsp;&nbsp;◦ Amazon Machine Images (AMI)<br>&nbsp;&nbsp;◦ Elastic Block Store (EBS)<br>&nbsp;&nbsp;◦ Security Groups<br>• Các phương pháp remote SSH vào EC2<br>• Tìm hiểu Elastic IP | 14/08/2025 | 15/08/2025 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| **Thứ 6** | • **Thực hành:**<br>&nbsp;&nbsp;◦ Tạo EC2 instance<br>&nbsp;&nbsp;◦ Kết nối SSH<br>&nbsp;&nbsp;◦ Gắn EBS volume<br>&nbsp;&nbsp;◦ Cấu hình Security Groups | 15/08/2025 | 15/08/2025 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

## Kết quả đạt được tuần 1

* Hiểu AWS là gì và nắm được các nhóm dịch vụ cơ bản: 
  * **Compute:** EC2, Lambda, ECS
  * **Storage:** S3, EBS, EFS
  * **Networking:** VPC, CloudFront, Route 53 
  * **Database:** RDS, DynamoDB
  * **Security:** IAM, WAF

* Đã tạo và cấu hình AWS Free Tier account thành công.

* Làm quen với AWS Management Console và biết cách tìm, truy cập, sử dụng dịch vụ từ giao diện web.

* Cài đặt và cấu hình AWS CLI trên máy tính bao gồm:
  * Access Key ID
  * Secret Access Key
  * Region mặc định (ap-southeast-1)
  * Output format (json)

* Sử dụng AWS CLI để thực hiện các thao tác cơ bản như:
  * Kiểm tra thông tin tài khoản & cấu hình: `aws sts get-caller-identity`
  * Lấy danh sách region: `aws ec2 describe-regions`
  * Xem dịch vụ EC2: `aws ec2 describe-instances`
  * Tạo và quản lý key pair: `aws ec2 create-key-pair`
  * Kiểm tra thông tin dịch vụ đang chạy

* Có khả năng kết nối giữa giao diện web và CLI để quản lý tài nguyên AWS song song.
  
* Cấu trúc các lớp trên đám mây:
  * **Data Centers:** Trung tâm dữ liệu vật lý
  * **Availability Zones (AZ):** Bao gồm một hoặc nhiều trung tâm dữ liệu, tối thiểu 2 data centers và được nối bằng đường truyền tốc độ cao
  * **Regions:** Tối thiểu 3 AZs per region, được phân bố địa lý
  * **Edge Locations:** Mạng lưới thứ cấp để chạy các dịch vụ mạng biên
  
  **Dịch vụ mạng biên chính hay sử dụng ở Việt Nam:**
  * **CloudFront (CDN):** Tăng tốc độ truy cập nội dung
  * **Web Application Firewall (WAF):** Đóng vai trò tạo ra lớp bảo vệ để tránh tấn công DDoS
  * **Route 53 (DNS service):** Tạo domain cho ứng dụng của mình, gắn SSL certificate

* Cách tối ưu hóa chi phí:
  * Lựa chọn cấu hình tài nguyên tính toán và nơi lưu trữ phù hợp với nhu cầu thực tế
  * Không chạy full cấu hình khi không cần thiết
  * Sử dụng Free Tier để học tập và thử nghiệm
  * Tắt/xóa tài nguyên không sử dụng để tiết kiệm chi phí
