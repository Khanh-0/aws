---
title: "Worklog Tuần 1"
date: 2024-01-01T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud Journey.
* Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- |-------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|-----------------| ----------------------------------------- |
| 2   | - Làm quen với các thành viên FCJ <br> - Đọc và lưu ý các nội quy, quy định tại đơn vị thực tập                                                             | 08/09/2025   | 08/09/2025      |
| 3   | - Tìm hiểu AWS và các loại dịch vụ <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + ... <br>            | 09/09/2025   | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tạo AWS Free Tier account <br> - Tìm hiểu AWS Console & AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo AWS account <br>&emsp;                             | 10/09/2025   | 10/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu các loại budget cơ bản: <br>&emsp; + Cost budget <br>&emsp; + Usage budget <br>&emsp; + Saving plans budget <br>&emsp; + Reservation budget <br> | 11/09/2025   | 11/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Tìm hiểu và thực hành:** <br>&emsp; + AWS support packages <br>&emsp; + Change AWS support packages <br>&emsp; + Manage support request                 | 12/09/2025   | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 1:

* Hiểu AWS là gì và nắm được các nhóm dịch vụ cơ bản: 
* Compute:Dùng để chạy ứng dụng, xử lý dữ liệu, tạo môi trường máy chủ ảo hoặc container.
    Một số dịch vụ chính:
    EC2 (Elastic Compute Cloud): Máy chủ ảo chạy trên AWS (giống VPS nhưng linh hoạt hơn).
    Lambda: Chạy code serverless, không cần quản lý server, trả phí theo số lần chạy.
    ECS/EKS (Elastic Container Service / Elastic Kubernetes Service): Quản lý container (Docker/K8s).
    Elastic Beanstalk: Deploy app tự động (giống PaaS)
* Storage:Lưu dữ liệu, từ file nhỏ đến dữ liệu Big Data.
    S3 (Simple Storage Service): Lưu trữ đối tượng (object storage) – dùng cho file, ảnh, video.
    EBS (Elastic Block Store): Lưu trữ dạng block (dùng kèm EC2 như ổ cứng).
    EFS (Elastic File System): Lưu trữ dạng file system, nhiều máy có thể cùng truy cập.
    Glacier: Lưu trữ lâu dài, giá rẻ (cho backup/archive).
* Networking:
    Kết nối các dịch vụ, bảo mật và phân phối ứng dụng.
    VPC (Virtual Private Cloud): Tạo mạng riêng ảo trong AWS (giống data center riêng).
    Route 53: Dịch vụ DNS, quản lý domain.
    CloudFront: CDN phân phối nội dung nhanh hơn cho người dùng toàn cầu.
    ELB (Elastic Load Balancer): Cân bằng tải giữa nhiều server.
    API Gateway: Quản lý và bảo mật API.
* Database:
    Lưu trữ và quản lý dữ liệu có cấu trúc/phi cấu trúc.
    RDS (Relational Database Service): Quản lý CSDL quan hệ (MySQL, PostgreSQL, Oracle, SQL Server).
    Aurora: Database do AWS phát triển, tương thích MySQL/PostgreSQL, nhanh hơn RDS.
    DynamoDB: NoSQL Database (phi quan hệ), tốc độ cao, tự động scale.
    Redshift: Data warehouse để phân tích dữ liệu lớn.
    ElastiCache: Cache dữ liệu (Redis/Memcached).


* Đã tạo và cấu hình AWS Free Tier account thành công.

* Làm quen với AWS Management Console và biết cách tìm, truy cập, sử dụng dịch vụ từ giao diện web.

* Cài đặt và cấu hình AWS CLI trên máy tính bao gồm:
  * Access Key
  * Secret Key
  * Region mặc định
  

* Sử dụng AWS CLI để thực hiện các thao tác cơ bản như:

  * Kiểm tra thông tin tài khoản & cấu hình
  * Lấy danh sách region
  * Xem dịch vụ EC2
  * Tạo và quản lý key pair
  * Kiểm tra thông tin dịch vụ đang chạy
  


