---
title: "Unlocking Next-Generation AI Performance with Dynamic Resource Allocation on Amazon EKS & EC2 P6e-GB200"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---


# Mở khóa hiệu năng AI thế hệ mới với Dynamic Resource Allocation trên Amazon EKS và Amazon EC2 P6e-GB200

*Bài viết của Vara Bonthu, Nick Baker, và Chris Splinter - 02 tháng 9, 2025*

Sự tiến hóa nhanh chóng của AI "agentic" và các mô hình ngôn ngữ lớn (LLM), đặc biệt là các mô hình suy luận, đã tạo ra nhu cầu chưa từng có về tài nguyên tính toán. Các mô hình AI tiên tiến ngày nay trải dài từ hàng trăm tỷ đến nghìn tỷ tham số và đòi hỏi sức mạnh tính toán khổng lồ, bộ nhớ lớn và liên kết tốc độ cao để hoạt động hiệu quả.

Các tổ chức phát triển ứng dụng cho xử lý ngôn ngữ tự nhiên, mô phỏng khoa học, tạo nội dung 3D và suy luận đa phương thức cần hạ tầng có thể mở rộng từ các mô hình tỷ tham số ngày nay đến biên giới nghìn tỷ tham số trong tương lai mà vẫn giữ hiệu suất.

Trong bài viết này, chúng ta sẽ khám phá cách **[Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2/) [P6e-GB200 UltraServers](https://aws.amazon.com/ec2/instance-types/p6/)** mới thay đổi khối lượng công việc AI phân tán qua tích hợp liền mạch với Kubernetes.

AWS giới thiệu các UltraServers EC2 P6e-GB200 để đáp ứng nhu cầu ngày càng tăng về huấn luyện và suy luận mô hình AI quy mô lớn. Chúng đại diện cho một bước đột phá kiến trúc đáng kể cho khối lượng công việc AI phân tán. Hơn nữa, việc ra mắt EC2 P6e-GB200 UltraServer bao gồm hỗ trợ **[Amazon EKS (Elastic Kubernetes Service)](https://aws.amazon.com/eks/)**, cung cấp môi trường nguyên gốc Kubernetes để triển khai và mở rộng từ các mô hình hàng trăm tỷ đến nghìn tỷ tham số khi bối cảnh AI tiếp tục phát triển.

---

## Sức mạnh phía sau P6e-GB200: kiến trúc NVIDIA GB200 Grace Blackwell

Ở trung tâm của các UltraServer EC2 P6e-GB200 là **[NVIDIA GB200 Grace Blackwell Superchip](https://www.nvidia.com/en-us/data-center/gb200-nvl72/)**, tích hợp hai GPU NVIDIA Blackwell và một CPU NVIDIA Grace. Ngoài ra, nó cung cấp kết nối NVLink-Chip-to-Chip (C2C) giữa các thành phần này, đem lại **900 GB/s** băng thông hai chiều, nhanh hơn đáng kể so với các giao diện PCIe truyền thống.

Khi được triển khai ở quy mô rack, các UltraServer EC2 P6e-GB200 tham gia vào kiến trúc **[NVL72](https://developer.nvidia.com/blog/nvidia-contributes-nvidia-gb200-nvl72-designs-to-open-compute-project/)** của NVIDIA, tạo ra các miền bộ nhớ đồng nhất (memory-coherent domains) đến 72 GPU.

Công nghệ **[NVLink](https://www.nvidia.com/en-us/data-center/nvlink/)** thế hệ năm hỗ trợ giao tiếp GPU-to-GPU giữa các máy chủ riêng biệt trong cùng miền lên tới **1.8 TB/s mỗi GPU**. Yếu tố then chốt cho hiệu suất này là mạng **[Elastic Fabric Adapter (EFAv4)](https://aws.amazon.com/hpc/efa/)**, cung cấp tổng băng thông mạng lên đến **28.8 Tbps** cho mỗi UltraServer.

EFA kết hợp với **NVIDIA GPUDirect RDMA** cho phép giao tiếp GPU-to-GPU giữa các máy chủ với độ trễ thấp, vượt qua hệ điều hành. Điều này đảm bảo rằng fabric GPU phân tán hoạt động với hiệu suất gần như bộ nhớ cục bộ giữa các node.

Đây là bước tiến đáng kể so với các UltraServer EC2 P6-B200 trước đây, vốn chỉ cung cấp tối đa 8 GPU B200 Blackwell trên nền tảng x86 dùng PCIe. P6e-GB200 nâng cấp kiến trúc bằng cách cung cấp bộ nhớ thực sự thống nhất qua các rack --- một yêu cầu quan trọng để huấn luyện và vận hành mô hình nghìn tỷ tham số một cách hiệu quả.

![Amazon EC2 P6e-GB200 UltraServers](/images/p6e-ultraserver.jpg)
*Hình 1: Amazon EC2 P6e-GB200 UltraServers*

---

## Hiểu kiến trúc UltraServer EC2 P6e-GB200

Một UltraServer EC2 P6e-GB200 **không phải là một instance EC2 đơn lẻ**. Thay vào đó, nó gồm nhiều instance EC2 được kết nối với nhau để hoạt động như một thực thể hợp nhất:

- **u-p6e-gb200x36**: Gồm 36 GPU phân phối trên 9 instance EC2
- **u-p6e-gb200x72**: Gồm 72 GPU phân phối trên 18 instance EC2

Mỗi instance P6e-GB200 cung cấp 4 GPU NVIDIA Blackwell. Do đó:

- Một UltraServer **u-p6e-gb200x36** là 9 instance (9 × 4 = 36 GPU)
- Một UltraServer **u-p6e-gb200x72** là 18 instance (18 × 4 = 72 GPU)

Trong Amazon EKS, mỗi instance EC2 xuất hiện dưới dạng một node Kubernetes riêng biệt, nhưng EKS hiểu vị trí topology và xử lý chúng như một phần của cùng UltraServer thông qua định tuyến aware topology.

---

## Tích hợp P6e-GB200 UltraServers với Amazon EKS

Nhóm Amazon EKS đã hợp tác chặt chẽ với NVIDIA từ đầu để thiết lập yêu cầu tích hợp P6e-GB200 với các node làm việc và control plane Kubernetes. Dựa trên các yêu cầu đó, chúng tôi phát triển **[AMI (Amazon Machine Images)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)** dành cho ARM64 **[Amazon Linux 2023](https://aws.amazon.com/linux/amazon-linux-2023/)** với flavor NVIDIA.

Chúng tôi cũng đóng gói sẵn các binary cho **[Internal Node Memory Exchange/Management Service (IMEX)](https://docs.nvidia.com/multi-node-nvlink-systems/imex-guide/overview.html)** và cài sẵn phiên bản driver NVIDIA cần thiết.

Hơn nữa, Amazon EKS đã nhanh chóng hỗ trợ **[Dynamic Resource Allocation (DRA)](https://kubernetes.io/docs/concepts/scheduling-eviction/dynamic-resource-allocation/)** cho người dùng bắt đầu từ phiên bản Kubernetes 1.33 trên EKS (tính năng này hiện vẫn là beta trong Kubernetes gốc).

Các instance đã được kiểm tra với NVLink qua IMEX cũng như qua EFA, để đạt được luồng dữ liệu tối ưu trong và giữa các UltraServer. Trong thử nghiệm nội bộ, chúng tôi sử dụng thư viện **NVIDIA Collective Communications Library (NCCL)** của NVIDIA, giúp trừu tượng hóa quyết định cấp giao vận từ lớp ứng dụng.

---

## Thách thức: chạy khối lượng công việc AI phân tán trên Kubernetes

Việc triển khai các workload GPU bó chặt (tightly coupled) qua nhiều node truyền thống gặp nhiều thách thức trong Kubernetes. Cách cấp tài nguyên truyền thống của Kubernetes giả định phần cứng gắn liền với từng node, làm khó quản lý tài nguyên GPU và kết nối bộ nhớ đồng nhất (memory-coherent interconnects) giữa các node.

Điều này phổ biến với các workload huấn luyện quy mô lớn như LLM hoặc mô hình thị giác máy tính cần nhiều GPU hoạt động song song.

Hãy xem cách tiếp cận truyền thống khi yêu cầu GPU trong một pod:

```yaml
resources:
  limits:
    nvidia.com/gpu: 2
```

Cách tiếp cận tĩnh này hoạt động tốt cho GPU cục bộ nhưng không thể biểu diễn topology kết nối phức tạp hoặc kênh giao tiếp GPU-to-GPU cần thiết cho các framework huấn luyện phân tán.

---

## Giải pháp: Kubernetes DRA và IMEX

Để giải quyết những thách thức này, Kubernetes giới thiệu **DRA (Dynamic Resource Allocation)**, một framework mở rộng Kubernetes vượt ra ngoài CPU và bộ nhớ truyền thống để xử lý các topologies phần cứng phức tạp.

Amazon EKS đã kích hoạt DRA trong Kubernetes phiên bản 1.33, cung cấp khả năng quản lý topology GPU tinh vi mà trước đây không thể thực hiện được bằng cấp tài nguyên GPU truyền thống.

### Cách DRA giải quyết vấn đề cấp GPU truyền thống

Khác với mô hình tài nguyên tĩnh (ví dụ `nvidia.com/gpu: 2`) -- trong đó bạn yêu cầu một số GPU cố định mà không quan tâm topology -- DRA cho phép ứng dụng mô tả yêu cầu tài nguyên theo cách khai báo qua **ComputeDomain** và **ResourceClaims**.

Đây là sự chuyển đổi căn bản, giúp Kubernetes đưa ra quyết định cấp tài nguyên thông minh dựa vào topology thực tế, xem xét kết nối NVLink, băng thông bộ nhớ, và khoảng cách vật lý một cách tự động.

Quan trọng nhất, DRA ẩn đi các cấu hình thủ công phức tạp như cài đặt dịch vụ IMEX, quản lý partition NVLink, và khởi tạo phần cứng cấp thấp --- những việc này nếu không sẽ đòi hỏi chuyên môn sâu về cụm GPU.

**NVIDIA DRA Driver** là phần kết nối quan trọng giữa API DRA của Kubernetes và phần cứng bên dưới. Nó bao gồm hai plugin kubelet chuyên dụng:

- `gpu-kubelet-plugin`: cho các tính năng cấp GPU nâng cao
- `compute-domain-kubelet-plugin`: điều phối primitives IMEX một cách tự động

Khi bạn tạo một **ComputeDomain** yêu cầu 36 GPU qua 9 instance EC2 (mỗi instance có 4 GPU Blackwell), hoặc 72 GPU qua 18 instance cho một UltraServer đầy đủ, hệ thống sẽ tự động:

- Triển khai daemon IMEX
- Thiết lập giao tiếp gRPC giữa các node
- Tạo miền bộ nhớ đồng nhất (memory-coherent domain) với ánh xạ cross-node
- Cung cấp các file thiết bị trong container

### Lên lịch aware topology & đồng nhất bộ nhớ (memory coherence)

Khi một node tham gia vào một cluster EKS, control plane sẽ tiếp nhận thông tin topology liên quan đến instance qua API topology EC2 và gán nhãn (labels) cho node Kubernetes khi tham gia:

- Mỗi node P6e-GB200 được tự động gán nhãn loại capacity block (`eks.amazonaws.com/capacityType=CAPACITY_BLOCK` và `eks.amazonaws.com/nodegroup=...`) và các nhãn topology mạng chi tiết (`topology.k8s.aws/network-node-layer-1` đến `network-node-layer-4`)
- Những nhãn này chỉ ra vị trí vật lý trong fabric mạng UltraServer

Khi **GPU Feature Discovery (GFD)** được bật trong **[NVIDIA GPU Operator](https://github.com/NVIDIA/gpu-operator)**, nó áp các nhãn clique (`nvidia.com/gpu.clique`) cho mỗi node để xác định GPU nào thuộc cùng một miền NVLink.

Những chiều topology này cho phép bạn thiết kế scheduling aware topology cho workload phân tán trên hoặc xuyên các nhóm node UltraServer.

**IMEX** là khả năng then chốt của các hệ thống hỗ trợ NVLink như GB200 cho phép GPU giữa các node khác nhau truy cập trực tiếp bộ nhớ của nhau qua NVLink. Khi một **kênh IMEX (IMEX channel)** được cấp qua Kubernetes và DRA thông qua một ComputeDomain, nó xuất hiện trong container như một file thiết bị (ví dụ `/dev/nvidia-caps-imex-channels/channel0`).

Điều này cho phép ứng dụng CUDA hoạt động như thể tất cả GPU nằm trên cùng một board.

Khả năng này đặc biệt quan trọng cho các framework huấn luyện phân tán như **[MPI](https://docs.open-mpi.org/en/v5.0.x/index.html)** và **[NCCL](https://developer.nvidia.com/nccl)**. Chúng giờ đây có thể đạt hiệu suất gần như mức "bare-metal" qua các ranh giới node mà không cần cấu hình tùy chỉnh hay thay đổi mã.

Công nghệ **[NVLink 5.0](https://www.nvidia.com/en-us/data-center/nvlink/)** cung cấp nền tảng băng thông để vận hành các kênh này với **1.8 TB/s** hai chiều mỗi GPU.

Điều này cho phép thực thi các miền tính toán (compute domains) đồng nhất bộ nhớ xuyên rack --- tạo nền tảng cho hệ thống AI đa node chạy thời gian thực. Trong kiến trúc NVL72, lên đến 72 GPU có thể kết nối trong một miền NVLink đồng nhất bộ nhớ.

GPU được tổ chức thành các **clique** dựa trên kết nối vật lý qua NVSwitches, với mọi GPU trong cùng một node chắc chắn thuộc cùng một clique và chia sẻ cùng Cluster UUID.

Khi GFD được bật, nó gán nhãn `nvidia.com/gpu.clique` cho mỗi node chứa ID miền NVL và ID clique (ví dụ `cluster-abc.0`), cho phép người dùng thiết kế scheduling aware topology sử dụng node affinity rules.

Khi lên lịch công việc huấn luyện qua 9 instance của UltraServer u-p6e-gb200x36 hoặc 18 instance u-p6e-gb200x72, kube-scheduler với các affinity rule hợp lý đảm bảo rằng tất cả node thuộc cùng một miền NVLink để đạt băng thông tối đa.

Mặc dù NVLink cung cấp băng thông siêu cao trong cùng miền vật lý, mạng EFA đảm bảo giao tiếp giữa các UltraServers khác nhau có độ trễ thấp và throughput cao. Khả năng RDMA của EFA kết hợp với GPUDirect cho phép các GPU giao tiếp trực tiếp xuyên node mà CPU không can thiệp, tạo ra kiến trúc lai trong đó giao tiếp nội UltraServer dùng NVLink còn giữa các UltraServer dùng EFA.

Điều này làm cho P6e-GB200 phù hợp cho huấn luyện mô hình khổng lồ có thể mở rộng từ triển khai đơn rack đến cụm siêu máy tính đa rack trong khi vẫn giữ được đặc tính hiệu suất tối ưu ở mọi quy mô.

### Quy trình lên lịch workload với DRA

Lưu đồ dưới đây minh họa cách Kubernetes DRA tích hợp với công nghệ NVIDIA GB200 IMEX để triển khai khối lượng công việc huấn luyện AI phân tán trên nhiều node.

Khi một pod yêu cầu 8 GPU để huấn luyện phân tán với các quy tắc affinity được cấu hình chính xác, hệ thống sẽ điều phối việc triển khai thông qua một quy trình phối hợp:

1. Người dùng chỉ định các pod cụ thể nhắm mục tiêu theo affinity theo node (`nvidia.com/gpu.clique`)
2. Kube-scheduler đặt các pod dựa trên các ràng buộc về affinity này
3. Các thành phần DRA xử lý việc quản lý tài nguyên và phối hợp giữa các node
4. Driver NVIDIA quản lý phân bổ GPU và điều phối IMEX
5. Dịch vụ IMEX đảm bảo tính nhất quán của bộ nhớ giữa các node thông qua giao tiếp gRPC

Kết quả là triển khai liền mạch trên hai node (mỗi node 4 GPU) trong cùng một miền NVLink, cho phép giao tiếp băng thông cao, độ trễ thấp, điều cần thiết cho khối lượng công việc huấn luyện AI quy mô lớn.

![DRA Workflow](/images/Blog/Blog_2/bl2_2.png)

---

## Cách sử dụng P6e-GB200 với Kubernetes DRA trên Amazon EKS

Phần này hướng dẫn từng bước thiết lập cụm EKS với UltraServer EC2 P6e-GB200 để tận dụng các khả năng nói trên.

### Yêu cầu tiên quyết

Trước khi bắt đầu, hãy đảm bảo rằng bạn có các công cụ và quyền truy cập sau. Tham khảo **[EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/setting-up.html)** để biết hướng dẫn.

- Đã cài đặt **[AWS Command Line Interface (AWS CLI)](https://aws.amazon.com/cli/)**
- `eksctl` (phiên bản hỗ trợ EKS 1.33)
- `kubectl`
- `helm`
- Quyền truy cập **Capacity Blocks** EC2 cho các instance P6e-GB200

### Bước 1: Đặt trước capacity UltraServer P6e-GB200

{{% notice note %}}
UltraServers P6e-GB200 chỉ có sẵn thông qua **Capacity Blocks** dành cho machine learning (ML). Bạn phải **đặt trước UltraServer (không phải các instance riêng lẻ)** trước khi tạo cụm EKS.
{{% /notice %}}

Trong giao diện console AWS:

1. Vào **EC2 Console → Capacity Reservations → Capacity Blocks**
2. Chọn tab **UltraServers** (không phải Instances)
3. Chọn một trong:
   - `u-p6e-gb200x36` (36 GPU qua 9 instance)
   - `u-p6e-gb200x72` (72 GPU qua 18 instance)
4. Hoàn thành việc đặt trước cho khoảng thời gian mong muốn

### Bước 2: Tạo file cấu hình EKS cluster

Tạo file tên `cluster-config.yaml` với nội dung:

```yaml
# cluster-config.yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: p6e-cluster
  region: us-east-1
  version: '1.33'

iam:
  withOIDC: true

managedNodeGroups:
  - name: p6e-nodegroup
    amiFamily: AmazonLinux2023
    instanceType: p6e-gb200.36xlarge
    desiredCapacity: 9  # All 9 instances from the UltraServer (36 GPUs total)
    minSize: 9
    maxSize: 9
    labels:
      nvidia.com/gpu.present: "true"
    taints:
      - key: nvidia.com/gpu
        value: "true"
        effect: NoSchedule
    availabilityZones: ["us-east-1-dfw-2a"]
    
    # Enable EFA (mandatory for P6e-GB200 UltraServers)
    efaEnabled: true
    
    capacityReservation:
      enabled: true
      capacityReservationTarget:
        capacityReservationId: "cr-1234567890abcdef"  # Replace with your reservation ID
```

### Bước 3: Triển khai cụm EKS

```bash
eksctl create cluster -f cluster-config.yaml
```

Lệnh này sẽ tạo cụm EKS phiên bản 1.33 với 9 instance `p6e-gb200.36xlarge` từ UltraServer bạn đã đặt trước, với mạng EFA được bật để tối ưu giao tiếp GPU-to-GPU.

### Bước 4: Triển khai NVIDIA GPU Operator

GPU Operator của NVIDIA là thành phần thiết yếu cho instance GB200 vì nó quản lý toàn bộ vòng đời GPU --- bao gồm cấu hình runtime và các tính năng nâng cao như MIG (Multi-Instance GPU).

Với topology NVLink phức tạp trải qua nhiều node, GPU Operator quản lý tài nguyên GPU động, cấu hình MIG, và xử lý các mối quan hệ liên kết interconnect mà plugin tĩnh không thể xử lý.

```bash
# Add the NVIDIA GPU Operator Helm repository
helm repo add nvidia https://nvidia.github.io/gpu-operator
helm repo update

# Deploy the NVIDIA GPU Operator with custom values
cat <<EOF > gpu-operator-values.yaml
# gpu-operator-values.yaml
driver:
  enabled: false

mig:
  strategy: mixed

migManager:
  enabled: true
  env:
    - name: WITH_REBOOT
      value: "true"
  config:
    create: true
    name: custom-mig-parted-configs
    default: "all-disabled"
    data:
      config.yaml: |-
        version: v1
        mig-configs:
          all-disabled:
            - devices: all
              mig-enabled: false
          # P4DE profiles (A100 80GB)
          p4de-half-balanced:
            - devices: [0, 1, 2, 3]
              mig-enabled: true
              mig-devices:
                "1g.10gb": 2
                "2g.20gb": 1
                "3g.40gb": 1
            - devices: [4, 5, 6, 7]
              mig-enabled: false

devicePlugin:
  enabled: true
  config:
    name: ""
    create: false
    default: ""

toolkit:
  enabled: false

nfd:
  enabled: true

gfd:
  enabled: true

dcgmExporter:
  enabled: true
  serviceMonitor:
    enabled: true
    interval: 15s
    honorLabels: false
    additionalLabels:
      release: kube-prometheus-stack

daemonsets:
  tolerations:
    - key: "nvidia.com/gpu"
      operator: "Exists"
      effect: "NoSchedule"
  nodeSelector:
    accelerator: nvidia
  priorityClassName: system-node-critical
EOF

# Install GPU Operator using values file
helm install gpu-operator nvidia/gpu-operator \
  --namespace gpu-operator \
  --create-namespace \
  --version v25.3.1 \
  --values gpu-operator-values.yaml
```

### Bước 5: Cài đặt NVIDIA DRA Driver

Driver DRA của NVIDIA là thành phần thiết yếu cho UltraServer P6e-GB200 vì nó cung cấp các khả năng vượt ra ngoài plugin GPU truyền thống.

Mặc dù plugin NVIDIA Device Plugin chuẩn chỉ expose các GPU riêng lẻ như tài nguyên đếm được (`nvidia.com/gpu: 2`), DRA Driver mở rộng hai khả năng quan trọng:

1. **Quản lý ComputeDomain**: DRA Driver quản lý các ComputeDomain --- là abstraction cho các triển khai Multi-Node NVLink (MNNVL)
2. **Cấp GPU nâng cao**: Ngoài việc đếm GPU, DRA Driver cho phép cấp cấu hình GPU động, các thiết bị MIG, và scheduling aware topology

DRA Driver bao gồm hai plugin kubelet:

- `gpu-kubelet-plugin`: cho các chức năng cấp GPU nâng cao
- `compute-domain-kubelet-plugin`: điều phối ComputeDomain

Tạo file `values.yaml` để triển khai **[NVIDIA DRA Driver](https://github.com/NVIDIA/k8s-dra-driver-gpu)**:

```yaml
# values.yaml
---
nvidiaDriverRoot: /
gpuResourcesEnabledOverride: true  # Required to deploy GPU and MIG deviceclasses

resources:
  gpus:
    enabled: true  # set to false to disable experimental gpu support
  computeDomains:
    enabled: true

controller:
  nodeSelector: null
  affinity: null
  tolerations: []

kubeletPlugin:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: "nvidia.com/gpu.present"
                operator: In
                values:
                  - "true"
  tolerations:
    - key: "nvidia.com/gpu"
      operator: Exists
      effect: NoSchedule
```

Sau đó cài đặt trình điều khiển NVIDIA DRA:

```bash
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
helm repo update

helm install nvidia-dra-driver-gpu nvidia/nvidia-dra-driver-gpu \
  --version="25.3.0-rc.4" \
  --namespace nvidia-dra-driver-gpu \
  --create-namespace \
  -f values.yaml
```

Sau khi cài, DRA Driver sẽ tạo ra các tài nguyên **DeviceClass** giúp Kubernetes hiểu và cấp ComputeDomain. Điều này giúp việc quản lý topology nâng cao cho workload AI phân tán trên UltraServer EC2 P6e-GB200 trở nên khả thi.

### Bước 6: Xác minh tài nguyên DRA

Kiểm tra xem các tài nguyên DRA đã khả dụng chưa:

```bash
kubectl api-resources | grep resource.k8s.io/v1beta1

# Output:
# deviceclasses                    resource.k8s.io/v1beta1        false        DeviceClass
# resourceclaims                   resource.k8s.io/v1beta1        true         ResourceClaim
# resourceclaimtemplates           resource.k8s.io/v1beta1        true         ResourceClaimTemplate
# resourceslices                   resource.k8s.io/v1beta1        false        ResourceSlice

kubectl get deviceclasses

# Output:
# NAME                              CAPACITY   ALLOCATABLE   ALLOCATED
# compute-domain-daemon.nvidia.com  36         36            0
# gpu.nvidia.com                    0          0             0
# mig.nvidia.com                    0          0             0
```

---

## Xác thực cấp kênh IMEX

Khi GPU Operator và DRA driver đã cấu hình, bạn có thể tạo các kênh IMEX để cho phép truy cập bộ nhớ trực tiếp giữa GPU xuyên các node. Ví dụ sau minh họa cách một tài nguyên ComputeDomain tự cấp phát hạ tầng IMEX cần thiết:

Tạo file `imex-channel-injection.yaml`:

```yaml
# filename: imex-channel-injection.yaml
---
apiVersion: resource.nvidia.com/v1beta1
kind: ComputeDomain
metadata:
  name: imex-channel-injection
spec:
  numNodes: 1
  channel:
    resourceClaimTemplate:
      name: imex-channel-0
---
apiVersion: v1
kind: Pod
metadata:
  name: imex-channel-injection
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: nvidia.com/gpu.clique
                operator: Exists
  containers:
    - name: ctr
      image: ubuntu:22.04
      command: ["bash", "-c"]
      args: ["ls -la /dev/nvidia-caps-imex-channels; trap 'exit 0' TERM; sleep 9999 & wait"]
      resources:
        claims:
          - name: imex-channel-0
  resourceClaims:
    - name: imex-channel-0
      resourceClaimTemplateName: imex-channel-0
```

YAML này tạo một tài nguyên ComputeDomain và tham chiếu nó từ một pod. Controller ComputeDomain tự động tạo ResourceClaimTemplate, pod sử dụng để truy cập kênh IMEX. Ở phía sau, việc này kích hoạt việc triển khai daemon IMEX trên node được chọn, và tạo miền IMEX một cách động thay vì thiết lập tĩnh trước.

**Áp dụng và xác thực:**

Bạn sẽ thấy pod như `imex-channel-injection-...` trong trạng thái Running.

```bash
kubectl apply -f imex-channel-injection.yaml

# Confirm the pod that runs to configure the compute domain
kubectl get pods -n nvidia-dra-driver-gpu -l resource.nvidia.com/computeDomain

# Output:
# NAME                                   READY   STATUS    RESTARTS      AGE
# imex-channel-injection-zrrlw-b6dqx     1/1     Running   5 (2m34s ago) 4m5s

# Confirm the IMEX channel is created
kubectl logs imex-channel-injection

# Output:
# total 0
# drwxr-xr-x. 2 root root  60 Apr 22 00:15 .
# drwxr-xr-x. 6 root root 380 Apr 22 00:15 ..
# crw-rw-rw-. 1 root root 241, 0 Apr 22 00:15 channel0

# Show logs of the pod configuring IMEX for the compute domain
kubectl logs -n nvidia-dra-driver-gpu -l resource.nvidia.com/computeDomain --tail=-1

# Output:
# /etc/nvidia-imex/nodes_config.cfg:
# 192.168.56.245
# IMEX Log initializing at: 4/22/2025 00:14:21.228
# [Apr 22 2025 00:14:21] [INFO] [tid 43] IMEX version 570.133.20 is running with the following configuration options
# [Apr 22 2025 00:14:21] [INFO] [tid 43] Logging level = 4
# [Apr 22 2025 00:14:21] [INFO] [tid 43] Logging file name/path = /var/log/nvidia-imex.log
# [Apr 22 2025 00:14:21] [INFO] [tid 43] Append to log file = 0
# [Apr 22 2025 00:14:21] [INFO] [tid 43] Max Log file size = 1024 (MBs)
# [Apr 22 2025 00:14:21] [INFO] [tid 43] Use Syslog file = 0
# [Apr 22 2025 00:14:21] [INFO] [tid 43] IMEX Library communication bind interface =
# [Apr 22 2025 00:14:21] [INFO] [tid 43] IMEX library communication bind port = 50000
# [Apr 22 2025 00:14:21] [INFO] [tid 43] Identified this node as ID 0, using bind IP of '196.181.26.911', and network interface of enp4s0
# [Apr 22 2025 00:14:21] [INFO] [tid 43] nvidia-imex persistence file /var/run/nvidia-imex/persist.dat does not exist. Assuming no previous importers.
# [Apr 22 2025 00:14:21] [INFO] [tid 43] NvGpu Library version matched with GPU Driver version
# [Apr 22 2025 00:14:21] [INFO] [tid 70] Started processing of incoming messages.
# [Apr 22 2025 00:14:21] [INFO] [tid 71] Started processing of incoming messages.
# [Apr 22 2025 00:14:21] [INFO] [tid 72] Started processing of incoming messages.
# [Apr 22 2025 00:14:21] [INFO] [tid 43] Creating gRPC channels to all peers (nPeers = 1).
# [Apr 22 2025 00:14:21] [INFO] [tid 73] Started processing of incoming messages.
# [Apr 22 2025 00:14:21] [INFO] [tid 43] IMEX_WAIT_FOR_QUORUM != FULL, continuing initialization without waiting for connections to all nodes.
# [Apr 22 2025 00:14:21] [INFO] [tid 43] GPU event successfully subscribed
# [Apr 22 2025 00:14:21] [INFO] [tid 74] Connection established to node 0 with ip address 192.168.56.245. Number of times connected: 1
```

Kết quả log sẽ cho biết thông tin khởi tạo IMEX, thiết lập gRPC giữa nodes, và xác nhận rằng miền bộ nhớ đồng nhất được kích hoạt. Điều này cho thấy GPU bộ nhớ từ các node khác nhau trong UltraServer giờ có thể truy cập trực tiếp qua NVLink. Kết quả cuối là hiệu năng chưa từng có cho workload AI phân tán.

### Giao tiếp IMEX đa node trong thực tế

Để minh họa cách driver DRA điều phối giao tiếp GPU xuyên node, phần tiếp theo triển khai một benchmark MPI đa node sử dụng kênh IMEX cho truyền bộ nhớ GPU-to-GPU băng thông cao xuyên các node EC2 P6e-GB200 UltraServer.

#### Triển khai Job MPI đa node

Tạo file yaml `nvbandwidth-test-job.yaml`:

```yaml
# nvbandwidth-test-job.yaml
---
apiVersion: resource.nvidia.com/v1beta1
kind: ComputeDomain
metadata:
  name: nvbandwidth-test-compute-domain
spec:
  numNodes: 2  # Request 2 nodes for cross-node testing
  channel:
    resourceClaimTemplate:
      name: nvbandwidth-test-compute-domain-channel
---
apiVersion: kubeflow.org/v2beta1
kind: MPIJob
metadata:
  name: nvbandwidth-test
spec:
  slotsPerWorker: 4  # 4 GPUs per worker node
  launcherCreationPolicy: WaitForWorkersReady
  mpiReplicaSpecs:
    Worker:
      replicas: 2  # 2 worker nodes
      template:
        spec:
          containers:
            - image: ghcr.io/nvidia/k8s-samples:nvbandwidth-v0.7-8d103163
              name: mpi-worker
              resources:
                limits:
                  nvidia.com/gpu: 4  # Request 4 GPUs per worker
                claims:
                  - name: compute-domain-channel  # Link to IMEX channel
          resourceClaims:
            - name: compute-domain-channel
              resourceClaimTemplateName: nvbandwidth-test-compute-domain-channel
```

Khi áp file này:

```bash
kubectl apply -f nvbandwidth-test-job.yaml
```

1. **ComputeDomain creation and node selection:** Trình điều khiển DRA ngay lập tức bắt đầu sắp xếp thiết lập nhiều node:
   a. Xác định 2 node có GPU GB200 có sẵn
   b. Xác minh các node thuộc cùng một domain NVLink
   c. Tạo tài nguyên ComputeDomain resource

2. **IMEX domain establishment:** DRA tự động hóa:
   a. Triển khai nhóm daemon IMEX trên cả hai node đã chọn
   b. Định cấu hình các kênh cross-node gRPC
   c. Thiết lập shared memory mappings giữa các GPU

Kết quả của experiment chứng minh DRA biến cụm GPU đa node thành tài nguyên hợp nhất, cho phép huấn luyện LLM trải khắp các node UltraServer với truy cập bộ nhớ GPU native trong khi vẫn giữ hiệu suất tối ưu. Tất cả 72 GPU trong một UltraServer u-p6e-gb200x72 hiển thị như một vùng nhớ thống nhất cho ứng dụng, và Kubernetes đảm trách toàn bộ orchestration IMEX phức tạp để đội ngũ dữ liệu chỉ cần tập trung vào mô hình, không phải hạ tầng.

---

## Kết luận

Amazon EC2 P6e-GB200 UltraServers trên Amazon EKS đại diện cho bước tiến lớn cho người dùng muốn huấn luyện và triển khai mô hình AI nghìn tỷ tham số ở quy mô. Sự kết hợp giữa GPU Grace Blackwell của NVIDIA với NVLink, hỗ trợ từ Amazon EKS, DRA và công cụ của NVIDIA, đã giúp AWS đưa tính toán AI ở cấp exascale vào phạm vi tiếp cận thông qua các pattern quản lý container quen thuộc.

Việc tích hợp các kênh IMEX và NVLink cho phép tạo cụm GPU đồng nhất bộ nhớ trải khắp các node và rack, phá vỡ giới hạn truyền thống của tính toán GPU cục bộ node. Cải tiến kiến trúc này mở ra các khả năng mới cho:

- Huấn luyện mô hình nền tảng hàng nghìn tỷ tham số
- Chạy AI đa phương thức với yêu cầu hiệu suất thời gian thực
- Triển khai pipeline suy luận phức tạp với độ trễ dưới một giây

Để bắt đầu với DRA trên Amazon EKS, bạn có thể tham khảo tài liệu **Amazon EKS AI/ML** để có hướng dẫn toàn diện, và khám phá dự án **AI on EKS**, cung cấp các ví dụ **DRA** bạn có thể thử nghiệm và triển khai trong môi trường của mình.

{{% notice warning %}}
**LƯU Ý BẢO MẬT:** Các cấu hình trình bày trong bài viết này chỉ là ví dụ cơ bản nhằm minh họa chức năng cốt lõi. Trong môi trường sản xuất, bạn nên bổ sung các kiểm soát bảo mật. Hãy liên hệ với nhóm tài khoản AWS của bạn để biết thêm về cách sử dụng P6e-GB200 trên Amazon EKS.
{{% /notice %}}

---

## Về tác giả

![Vara Bonthu](/images/Blog/Blog_2/blavt2_1.png)

**Vara Bonthu** là Chuyên gia SA chuyên về mã nguồn mở dẫn đầu Data trên EKS và AI trên EKS tại AWS, thúc đẩy các sáng kiến mã nguồn mở và hỗ trợ khách hàng từ nhiều tổ chức. Ông chuyên sâu về công nghệ mã nguồn mở, phân tích dữ liệu, AI/ML và Kubernetes, với kinh nghiệm sâu rộng trong phát triển, DevOps và kiến trúc.

---

![Chris Splinter](/images/Blog/Blog_2/blavt2_2.png)

**Chris Splinter** là Quản lý Sản phẩm cao cấp nhóm Amazon EKS, tập trung vào hỗ trợ khách hàng chạy workload AI với Kubernetes.

---

![Nick Baker](/images/Blog/Blog_2/blavt2_3.png)

**Nick Baker** là Kỹ sư phát triển phần mềm trong Nhóm Node Runtime trên Amazon EKS. Anh tập trung vào việc bổ sung hỗ trợ cho workload tăng tốc và cải thiện tính ổn định data-plane trên EKS.