---
title: "Worklog Tuần 2"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---



### Mục tiêu tuần 2:

* Nắm vững kiến thức về bảo mật mạng trong AWS (Security Group, NACL).
* Hiểu về kết nối VPC và các phương thức kết nối mạng.
* Tìm hiểu về cân bằng tải với AWS ELB và các loại Load Balancer.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu Security Group: <br>&emsp; + Khái niệm và đặc điểm stateful <br>&emsp; + Cách tạo và cấu hình rules <br>&emsp; + Áp dụng lên Elastic Network Interface | 11/08/2025 | 11/08/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Tìm hiểu Network ACL (NACL): <br>&emsp; + Khái niệm và đặc điểm stateless <br>&emsp; + Sự khác biệt giữa NACL và Security Group <br>&emsp; + Cơ chế đánh giá rules từ trên xuống | 12/08/2025 | 12/08/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - **Thực hành:** <br>&emsp; + Tạo Security Group cho EC2 instance <br>&emsp; + Cấu hình NACL cho subnet <br>&emsp; + So sánh hiệu quả bảo mật giữa 2 phương pháp <br> - Tìm hiểu VPC Flow Logs | 13/08/2025 | 13/08/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Tìm hiểu kết nối VPC: <br>&emsp; + VPC Peering và hạn chế <br>&emsp; + Transit Gateway <br>&emsp; + VPN Site-to-Site <br>&emsp; + AWS Direct Connect | 14/08/2025 | 14/08/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Tìm hiểu Elastic Load Balancing: <br>&emsp; + Khái niệm ELB và các loại <br>&emsp; + Application Load Balancer (ALB) <br>&emsp; + Network Load Balancer (NLB) <br>&emsp; + Sticky Session và Health Check <br> - **Thực hành:** <br>&emsp; + Tạo ALB để phân phối traffic <br>&emsp; + Cấu hình Target Group | 15/08/2025 | 15/08/2025 | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 2:

* Hiểu rõ về Security Group và NACL:
  * Security Group là tường lửa stateful, chỉ cho phép allow rules
  * NACL là tường lửa stateless, áp dụng cho subnet
  * Security Group ảnh hưởng đến từng instance, NACL ảnh hưởng đến nhiều máy chủ trong subnet

* Nắm được cách sử dụng VPC Flow Logs để giám sát lưu lượng IP trong VPC mà không capture nội dung gói tin.

* Hiểu về các phương thức kết nối VPC:
  * VPC Peering để kết nối 2 VPC, không hỗ trợ transitive routing
  * Transit Gateway để kết nối nhiều VPC và on-premises
  * VPN Site-to-Site với Virtual Private Gateway và Customer Gateway
  * AWS Direct Connect với độ trễ thấp (20-30ms)

* Nắm vững kiến thức về Elastic Load Balancing:
  * 4 loại: ALB, NLB, Classic LB, Gateway LB
  * ALB hoạt động ở Layer 7, hỗ trợ HTTP/HTTPS và path-based routing
  * NLB hoạt động ở Layer 4, hỗ trợ TCP/TLS và IP tĩnh
  * Tính năng Health Check, Sticky Session, Access Logs

* Thực hành thành công việc tạo và cấu hình:
  * Security Group cho EC2 với rules phù hợp
  * NACL cho subnet với thứ tự rules hợp lý
  * Application Load Balancer với Target Groups
  * Kiểm tra hoạt động của Load Balancer và phân phối traffic

* Có khả năng so sánh và lựa chọn giải pháp bảo mật, kết nối mạng phù hợp cho từng trường hợp cụ thể.