---
title: "Dynamic Kubernetes request right sizing with Kubecost"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---



# Dynamic Kubernetes Request Right Sizing with Kubecost

Trong bài viết này, chúng ta sẽ tìm hiểu cách sử dụng **Kubecost Amazon Elastic Kubernetes Service (Amazon EKS) add-on** để giảm chi phí hạ tầng và tối ưu hóa hiệu suất Kubernetes. Tính năng *Container Request Right Sizing* giúp đánh giá cách các yêu cầu container được cấu hình, phát hiện sự kém hiệu quả và tối ưu chúng thủ công hoặc tự động.

Chúng ta cũng sẽ xem cách đánh giá các đề xuất right sizing và thực hiện các cập nhật một lần hoặc tự động theo lịch trình, giúp môi trường Amazon EKS duy trì trạng thái tối ưu liên tục.



## Yêu cầu Container là gì?

Trong Kubernetes, một *container request* là mức CPU và bộ nhớ tối thiểu mà workload khai báo để scheduler có thể đặt pod lên node phù hợp.

- Scheduler sẽ tìm node có đủ tài nguyên chưa dùng.
- Khi pod được đặt lên node, tài nguyên đó được “giữ chỗ”, dù container có dùng hết hay không.
- Nếu yêu cầu quá cao → lãng phí tài nguyên và tăng chi phí.
- Yêu cầu còn ảnh hưởng đến lớp Quality-of-Service (QoS) và quyết định eviction.



## Kubecost Savings Insights

Kubecost Amazon EKS add-on cung cấp khả năng hiển thị chi tiết về các container đang yêu cầu tài nguyên quá mức.

Dashboard *Container Request Sizing* hiển thị:

- Danh sách container có thể tối ưu yêu cầu tài nguyên  
- Hiệu quả yêu cầu CPU/memory hiện tại  
- Mức sử dụng CPU/memory trung bình và tối đa  
- Ước tính chi phí tiết kiệm mỗi tháng  
- Tổng mức tiết kiệm tiềm năng cho toàn cluster  

![1](/images/Blog/Blog_1/bl1_1.png)
> *Hình 1: Container Request Right Sizing recommendations*

Right sizing đặc biệt hiệu quả trong môi trường dev, test và staging — nơi yêu cầu hiệu suất thường “dư thừa”.



## Tùy chỉnh đề xuất thay đổi kích thước

Kubecost cho phép điều chỉnh các đề xuất theo:

- Loại workload  
- Criticality  
- Mục tiêu vận hành  

Bạn có thể chọn các profile dựng sẵn như:

- `development`  
- `production`  
- `high availability`

Hoặc tự tạo profile riêng.

Các tham số điều chỉnh gồm:

- CPU/memory target utilization  
- Query window (48h, 7 ngày, …)
- Lọc workload theo label, namespace, controller  



## Hành động với đề xuất của Kubecost

### 1. Thay đổi kích thước một lần

**Resize Requests Now** trong dashboard giúp áp dụng ngay các request được đề xuất lên:

- Deployments  
- StatefulSets  
- Jobs  

Phản ánh đúng profile bạn đã chọn.
![2](/images/Blog/Blog_1/bl1_2.png)

> *Hình 2: Enable Resize Requests, and Enable Autoscaling*



### 2. Right sizing theo lịch trình

Chọn **Enable Autoscaling** để:

- Thiết lập job resize định kỳ  
- Cập nhật yêu cầu theo dữ liệu sử dụng gần đây  
- Duy trì request tối ưu theo thời gian  

Ví dụ:  
Job chạy mỗi 2 giờ, dựa trên dữ liệu 48h, target 80% CPU/memory.



### 3. Tự động hóa bằng Helm

Có thể enable tự động right sizing ngay trong bước cài đặt Kubecost bằng Helm.

Ví dụ cấu hình:

```yaml
clusterController:
  enabled: true
  actionConfigs:
    containerRightsize:
      filterConfig:
        - filter: |
            controllerKind:"deployment"
      schedule:
        start: "2024-08-20T00:00:00Z"
        frequencyMinutes: 120
        recommendationQueryWindow: "48h"
        targetUtilizationCPU: 0.8
        targetUtilizationMemory: 0.8
````

Cấu hình này:

* Chạy mỗi 2 giờ
* Nhắm mục tiêu mức sử dụng 80%
* Áp dụng cho tất cả Deployments
* Lý tưởng cho platform team muốn enforce best practices tự động


## Kết luận

Lợi ích thực tế từ Kubecost request sizing:

* Giảm **20–60%** chi phí compute trong môi trường non-production
* Tăng mức sử dụng node
* Lập lịch pod nhanh hơn
* Tối ưu hiệu suất và giảm bottleneck

Kubecost giúp bạn:

* Phát hiện workload kém hiệu quả
* Nhận đề xuất dựa trên dữ liệu
* Tùy chỉnh chiến lược tối ưu
* Áp dụng thủ công hoặc tự động

Nếu bạn chưa dùng Kubecost, hãy bắt đầu tại trang Get Started để cài đặt vào EKS cluster.



## Về tác giả

| ![Kai](/images/Blog/Blog_1/blavt1.png) | **Kai Wombacher**  
Giám đốc sản phẩm tại IBM Kubecost, chuyên về tối ưu Kubernetes chi phí lớn. |


| ![Jason](images/Blog/Blog_1/blavt2.png) | **Jason Janiak**  
Kiến trúc sư giải pháp đối tác tại AWS. |


| ![Mike](images/Blog/Blog_1/blavt3.png) | **Mike Stefaniak**  
Giám đốc cấp cao nhóm sản phẩm Amazon EKS tại AWS. |

