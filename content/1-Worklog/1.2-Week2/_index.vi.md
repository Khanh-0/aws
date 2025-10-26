---
title: "Worklog Tuần 2"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Hiểu về Amazon Virtual Private Cloud (VPC)
* Hiểu cách triển khai Amazon EC2 instances 
* Biết cách cấu hình AWS Site-to-Site VPN

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu về Amazon Virtual Private Cloud (Amazon VPC): <br>&emsp; + Subnets <br>&emsp; + Route Table <br>&emsp; + Internet Gateway <br>&emsp; + NAT Gateway <br> - **Thực hành:** <br>&emsp; + Tạo VPC <br>&emsp; + Tạo Subnet <br>&emsp; + Tạo Internet Gateway <br>&emsp; + Tạo Route Table <br>  | 15/09/2025   | 15/09/2025   | - <https://000003.awsstudygroup.com/vi/1-introduce/> <br> - <https://000003.awsstudygroup.com/vi/3-prerequisite/> |
| 3   | - Tìm hiểu Tường lửa trong VPC: <br>&emsp; + Security Group <br>&emsp; + Network ACLs <br>&emsp; + NetworkingVPC Resource Map <br> - **Thực hành:** <br>&emsp; + Tạo Security Group <br>&emsp; + Kích hoạt VPC Flow Logs <br>   | 16/09/2025   | 16/09/2025  | - <https://000003.awsstudygroup.com/vi/2-firewallinvpc/> <br> - <https://000003.awsstudygroup.com/vi/3-prerequisite/> |
| 4   |  - Triển khai Amazon EC2 Instances: <br>&emsp; + Triển khai Hạ tầng EC2 Production-Ready <br>&emsp; + Tính năng Production-Ready <br> - **Thực hành:** <br>&emsp; + Tạo máy chủ EC2 <br>&emsp; + Kiểm tra kết nối <br>&emsp; + Tạo NAT Gateway <br>&emsp; + Sử dụng Reachability Analyzer <br>&emsp; + Tạo EC2 Instance Connect Endpoint (Optional) <br>&emsp; + AWS Systems Manager Session Manager <br>&emsp; + CloudWatch Monitoring & Alerting <br> | 17/09/2025   | 17/09/2025    | <https://000003.awsstudygroup.com/vi/4-createec2server/> |
| 5   | - Cấu hình Site to Site VPN: <br>&emsp; + Tạo môi trường VPN <br>&emsp; + Cấu hình kết nối VPN <br>&emsp; + Cấu hình VPN bằng strongSwan với Transit Gateway (Tùy chọn) <br> - Dọn dẹp tài nguyên <br> - **Thực hành:** <br>&emsp; + Tạo VPC cho VPN <br>&emsp; + Tạo EC2 Instance <br>&emsp; + Tạo Virtual Private Gateway <br>&emsp; + Tạo Customer Gateway <br>&emsp; + Tạo kết nối VPN <br>&emsp; + Cấu hình Customer Gateway <br>&emsp; + Tùy chỉnh AWS VPN Tunnel <br>&emsp; + Cấu hình VPN Nâng cao <br>&emsp; + Troubleshooting VPN <br>&emsp; + Tạo Transit Gateway Attachment <br>&emsp; + Cấu hình Route Tables <br>| 18/09/2025   | 18/09/2025      | - <https://000003.awsstudygroup.com/vi/5-vpnsitetosite/> <br> - <https://000003.awsstudygroup.com/vi/6-cleanup/> |
| 6   | - Infrastructure as Code Templates: <br>&emsp; + Tự động hóa Triển khai VPC với Infrastructure as Code <br>&emsp; + Lợi ích của IaC cho Triển khai VPC <br>&emsp; + CloudFormation Template | 19/09/2025   | 19/09/2025      | <https://000003.awsstudygroup.com/vi/7-infrastructureascode/> |


### Kết quả đạt được tuần 2:

* Hiểu và triển khai được Amazon VPC: 
  * Subnets 
  * Route Table
  * Internet Gateway
  * NAT Gateway.

* Cấu hình thành công Security Group, Network ACLs, VPC Flow Logs.

* Tạo và kiểm thử EC2 Instance, áp dụng NAT Gateway, Reachability Analyzer, Instance Connect Endpoint, Session Manager, CloudWatch.

* Thiết lập Site-to-Site VPN: 
  * Tạo và cấu hình Virtual Private Gateway, Customer Gateway, VPN Connection, Route Tables
  * Thử nghiệm cấu hình strongSwan và troubleshooting.

* Làm quen với hạ tầng tự động hóa bằng Infrastructure as Code (CloudFormation Templates).