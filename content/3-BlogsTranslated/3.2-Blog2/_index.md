---

title: "Unlocking Next-Generation AI Performance with Dynamic Resource Allocation on Amazon EKS & EC2 P6e-GB200"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
----------------------

# Unlocking Next-Generation AI Performance with Dynamic Resource Allocation on Amazon EKS & EC2 P6e-GB200

*By Vara Bonthu, Nick Baker, and Chris Splinter – September 2, 2025*

The rapid evolution of agentic AI and large language models (LLMs), especially reasoning models, has created unprecedented demands for computational resources. Modern advanced AI models span hundreds of billions to trillions of parameters and require massive compute power, large memory, and high-speed interconnects to operate efficiently.

Organizations developing applications for natural language processing, scientific simulations, 3D content generation, and multimodal reasoning need infrastructure that can scale from today’s hundreds-of-billion-parameter models to future trillion-parameter boundaries while maintaining performance.

In this article, we explore how **[Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2/) [P6e-GB200 UltraServers](https://aws.amazon.com/ec2/instance-types/p6/)** transform distributed AI workloads through seamless integration with Kubernetes.

AWS introduces the EC2 P6e-GB200 UltraServers to meet the growing demand for large-scale AI model training and inference. They represent a significant architectural breakthrough for distributed AI workloads. Moreover, the launch of the EC2 P6e-GB200 UltraServer includes support for **[Amazon EKS (Elastic Kubernetes Service)](https://aws.amazon.com/eks/)**, providing a Kubernetes-native environment to deploy and scale from hundreds-of-billion-parameter models to trillions of parameters as AI landscapes continue to evolve.

---

## The Power Behind the P6e-GB200: NVIDIA GB200 Grace Blackwell Architecture

At the core of EC2 P6e-GB200 UltraServers is the **[NVIDIA GB200 Grace Blackwell Superchip](https://www.nvidia.com/en-us/data-center/gb200-nvl72/)**, which integrates two NVIDIA Blackwell GPUs and one NVIDIA Grace CPU. Additionally, it provides NVLink Chip-to-Chip (C2C) connectivity between these components, delivering **900 GB/s** bidirectional bandwidth—significantly faster than traditional PCIe interfaces.

When deployed at rack scale, EC2 P6e-GB200 UltraServers participate in NVIDIA’s **[NVL72](https://developer.nvidia.com/blog/nvidia-contributes-nvidia-gb200-nvl72-designs-to-open-compute-project/)** architecture, creating memory-coherent domains up to 72 GPUs.

Fifth-generation **[NVLink](https://www.nvidia.com/en-us/data-center/nvlink/)** enables GPU-to-GPU communication across hosts within the same domain at up to **1.8 TB/s per GPU**. A key enabler of this performance is the **[Elastic Fabric Adapter (EFAv4)](https://aws.amazon.com/hpc/efa/)** network, providing total network bandwidth up to **28.8 Tbps** per UltraServer.

EFA, combined with **NVIDIA GPUDirect RDMA**, enables GPU-to-GPU communication across hosts with low latency, bypassing the operating system. This ensures that the distributed GPU fabric operates with near-local memory performance across nodes.

This marks a significant improvement over previous EC2 P6-B200 UltraServers, which offered up to eight B200 Blackwell GPUs on x86 PCIe-based platforms. The P6e-GB200 upgrades the architecture by providing truly unified memory across racks—a critical requirement for training and operating trillion-parameter models efficiently.

![Amazon EC2 P6e-GB200 UltraServers](/images/p6e-ultraserver.jpg)
*Figure 1: Amazon EC2 P6e-GB200 UltraServers*

---

## Understanding EC2 P6e-GB200 UltraServer Architecture

An EC2 P6e-GB200 UltraServer **is not a single EC2 instance**. Instead, it consists of multiple EC2 instances connected to operate as a unified entity:

* **u-p6e-gb200x36**: 36 GPUs distributed across 9 EC2 instances
* **u-p6e-gb200x72**: 72 GPUs distributed across 18 EC2 instances

Each P6e-GB200 instance provides 4 NVIDIA Blackwell GPUs. Therefore:

* A **u-p6e-gb200x36** UltraServer = 9 instances (9 × 4 = 36 GPUs)
* A **u-p6e-gb200x72** UltraServer = 18 instances (18 × 4 = 72 GPUs)

In Amazon EKS, each EC2 instance appears as a separate Kubernetes node, but EKS understands topology location and treats them as part of the same UltraServer through topology-aware routing.

---

## Integrating P6e-GB200 UltraServers with Amazon EKS

The Amazon EKS team collaborated closely with NVIDIA from the outset to establish integration requirements for P6e-GB200 with Kubernetes worker and control plane nodes. Based on these requirements, we developed **[AMI (Amazon Machine Images)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)** for ARM64 **[Amazon Linux 2023](https://aws.amazon.com/linux/amazon-linux-2023/)** with NVIDIA flavor.

We also pre-packaged binaries for **[Internal Node Memory Exchange/Management Service (IMEX)](https://docs.nvidia.com/multi-node-nvlink-systems/imex-guide/overview.html)** and installed the required NVIDIA driver version.

Furthermore, Amazon EKS quickly supported **[Dynamic Resource Allocation (DRA)](https://kubernetes.io/docs/concepts/scheduling-eviction/dynamic-resource-allocation/)** for users starting from Kubernetes 1.33 on EKS (this feature is still in beta in vanilla Kubernetes).

Instances have been validated with NVLink via IMEX as well as via EFA to achieve optimal data flow within and between UltraServers. Internally, we leverage **NVIDIA Collective Communications Library (NCCL)** to abstract transport-level decisions from the application layer.

---

## Challenge: Running Distributed AI Workloads on Kubernetes

Deploying tightly coupled GPU workloads across multiple traditional nodes presents challenges in Kubernetes. Kubernetes’ traditional resource allocation assumes hardware tied to each node, making GPU resource management and memory-coherent interconnects across nodes difficult.

This is common for large-scale training workloads, such as LLMs or computer vision models requiring many GPUs to operate in parallel.

A traditional approach when requesting GPUs in a pod looks like this:

```yaml
resources:
  limits:
    nvidia.com/gpu: 2
```

This static approach works for local GPUs but cannot represent complex interconnect topologies or GPU-to-GPU communication channels required by distributed training frameworks.

---

## Solution: Kubernetes DRA and IMEX

To address these challenges, Kubernetes introduces **DRA (Dynamic Resource Allocation)**, an extension framework that goes beyond traditional CPU and memory to handle complex hardware topologies.

Amazon EKS enabled DRA in Kubernetes 1.33, providing sophisticated GPU topology management that static GPU allocation cannot achieve.

### How DRA Resolves Traditional GPU Allocation Limits

Unlike static resource models (e.g., `nvidia.com/gpu: 2`)—where you request a fixed number of GPUs without considering topology—DRA allows applications to declaratively describe resource requests via **ComputeDomain** and **ResourceClaims**.

This fundamental shift enables Kubernetes to make intelligent resource decisions based on actual topology, considering NVLink connectivity, memory bandwidth, and physical distance automatically.

Critically, DRA abstracts away complex manual configurations, such as IMEX setup, NVLink partition management, and low-level hardware initialization, which would otherwise require deep GPU cluster expertise.

The **NVIDIA DRA Driver** is the key connector between Kubernetes DRA APIs and underlying hardware, including two specialized kubelet plugins:

* `gpu-kubelet-plugin`: for advanced GPU allocation features
* `compute-domain-kubelet-plugin`: automatically orchestrates IMEX primitives

When creating a **ComputeDomain** requesting 36 GPUs across 9 EC2 instances (each with 4 Blackwell GPUs) or 72 GPUs across 18 instances for a full UltraServer, the system automatically:

* Deploys the IMEX daemon
* Establishes gRPC communication between nodes
* Creates a memory-coherent domain with cross-node mappings
* Exposes device files in containers

### Topology-aware Scheduling & Memory Coherence

When a node joins an EKS cluster, the control plane receives topology information via the EC2 topology API and labels Kubernetes nodes:

* Each P6e-GB200 node is automatically labeled with capacity block type (`eks.amazonaws.com/capacityType=CAPACITY_BLOCK` and `eks.amazonaws.com/nodegroup=...`) and detailed network topology labels (`topology.k8s.aws/network-node-layer-1` through `network-node-layer-4`)
* These labels indicate physical location within the UltraServer network fabric

When **GPU Feature Discovery (GFD)** is enabled in **[NVIDIA GPU Operator](https://github.com/NVIDIA/gpu-operator)**, it applies clique labels (`nvidia.com/gpu.clique`) to identify GPUs within the same NVLink domain.

This topology enables scheduling-aware workloads across or within UltraServer node groups.

**IMEX** is the core capability that allows GPUs across different nodes to directly access each other’s memory over NVLink. When an **IMEX channel** is provisioned via Kubernetes and DRA through a ComputeDomain, it appears in the container as a device file (e.g., `/dev/nvidia-caps-imex-channels/channel0`).

This allows CUDA applications to operate as if all GPUs were on the same board.

This capability is critical for distributed training frameworks like **[MPI](https://docs.open-mpi.org/en/v5.0.x/index.html)** and **[NCCL](https://developer.nvidia.com/nccl)**, now achieving near "bare-metal" performance across node boundaries without custom configuration or code changes.

**[NVLink 5.0](https://www.nvidia.com/en-us/data-center/nvlink/)** provides the bandwidth foundation to operate these channels at **1.8 TB/s** bidirectionally per GPU.

This enables memory-coherent compute domains across racks—foundational for real-time multi-node AI systems. In the NVL72 architecture, up to 72 GPUs can connect in a single memory-coherent NVLink domain.

GPUs are organized into **cliques** based on physical NVSwitch connections, with all GPUs in the same node belonging to the same clique and sharing a Cluster UUID.

With GFD enabled, `nvidia.com/gpu.clique` labels each node with NVL domain ID and clique ID (e.g., `cluster-abc.0`), enabling scheduling-aware topology using node affinity rules.

When scheduling training across 9 nodes of u-p6e-gb200x36 or 18 nodes of u-p6e-gb200x72, the kube-scheduler ensures all nodes belong to the same NVLink domain for maximum bandwidth.

While NVLink provides ultra-high bandwidth within a physical domain, EFA ensures low-latency, high-throughput communication between different UltraServers. EFA’s RDMA capability combined with GPUDirect allows GPUs to communicate directly across nodes, creating a hybrid architecture where intra-UltraServer uses NVLink and inter-UltraServer uses EFA.

This makes the P6e-GB200 ideal for training massive models that scale from single-rack deployments to multi-rack supercomputers while maintaining optimal performance characteristics at all scales.

### DRA Workload Scheduling Workflow

The diagram below illustrates how Kubernetes DRA integrates with NVIDIA GB200 IMEX to deploy distributed AI training workloads across nodes.

When a pod requests 8 GPUs for distributed training with properly configured affinity rules, the system orchestrates deployment through a coordinated process:

1. Users specify targeted pods by node affinity (`nvidia.com/gpu.clique`)
2. Kube-scheduler places pods according to these affinity constraints
3. DRA components manage resource allocation and node coordination
4. NVIDIA driver handles GPU allocation and IMEX orchestration
5. IMEX service ensures memory consistency across nodes via gRPC

The result is seamless deployment across two nodes (each with 4 GPUs) within the same NVLink domain, enabling high-bandwidth, low-latency communication essential for large-scale AI training workloads.

![DRA Workflow](/images/Blog/Blog_2/bl2_2.png)

---

## Using P6e-GB200 with Kubernetes DRA on Amazon EKS

This section provides step-by-step guidance to set up an EKS cluster with EC2 P6e-GB200 UltraServers to leverage the above capabilities.

### Prerequisites

Before starting, ensure you have the following tools and access. Refer to the **[EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/setting-up.html)**.

* Installed **[AWS CLI](https://aws.amazon.com/cli/)**
* `eksctl` (version supporting EKS 1.33)
* `kubectl`
* `helm`
* EC2 **Capacity Blocks** access for P6e-GB200 instances

### Step 1: Reserve UltraServer P6e-GB200 Capacity

{{% notice note %}}
P6e-GB200 UltraServers are available only through **Capacity Blocks** for machine learning (ML). You must **reserve the UltraServer (not individual instances)** before creating an EKS cluster.
{{% /notice %}}

In the AWS console:

1. Go to **EC2 Console → Capacity Reservations → Capacity Blocks**
2. Select the **UltraServers** tab (not Instances)
3. Choose one of:

   * `u-p6e-gb200x36` (36 GPUs across 9 instances)
   * `u-p6e-gb200x72` (72 GPUs across 18 instances)
4. Complete the reservation for the desired time period

### Step 2: Create EKS Cluster Configuration File

Create a file named `cluster-config.yaml` with the following content:

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

### Step 3: Deploy EKS Cluster

```bash
eksctl create cluster -f cluster-config.yaml
```

This command creates an EKS 1.33 cluster with 9 `p6e-gb200.36xlarge` instances from your reserved UltraServer, with EFA network enabled to optimize GPU-to-GPU communication.

### Step 4: Deploy NVIDIA GPU Operator

The NVIDIA GPU Operator is essential for GB200 instances as it manages the full GPU lifecycle—including runtime configuration and advanced features such as MIG.

With complex NVLink topology spanning multiple nodes, GPU Operator dynamically manages GPU resources, configures MIG, and handles interconnect relationships that static plugins cannot.

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

### Step 5: Install NVIDIA DRA Driver

The NVIDIA DRA driver is essential for P6e-GB200 UltraServers, providing capabilities beyond traditional GPU plugins.

While the standard NVIDIA Device Plugin exposes individual GPUs as countable resources (`nvidia.com/gpu: 2`), the DRA driver extends two important capabilities:

1. **ComputeDomain Management**: DRA Driver manages ComputeDomains—an abstraction for Multi-Node NVLink (MNNVL) deployments
2. **Advanced GPU Allocation**: Beyond counting GPUs, it allows dynamic GPU allocation, MIG devices, and scheduling-aware topology

The DRA driver includes two kubelet plugins:

* `gpu-kubelet-plugin`: for advanced GPU allocation functions
* `compute-domain-kubelet-plugin`: orchestrates ComputeDomain

Create a `values.yaml` for **[NVIDIA DRA Driver](https://github.com/NVIDIA/k8s-dra-driver-gpu)`:

```yaml
# values.yaml
---
nvidiaDriverRoot: /
gpuResourcesEnabledOverride: true  # Required to deploy GPU and MIG deviceclasses

resources:
  gpus:
    enabled: true
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
        node
```


Then install the NVIDIA DRA driver:

```bash
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
helm repo update

helm install nvidia-dra-driver-gpu nvidia/nvidia-dra-driver-gpu \
  --version="25.3.0-rc.4" \
  --namespace nvidia-dra-driver-gpu \
  --create-namespace \
  -f values.yaml
```

After installation, the DRA driver creates **DeviceClass** resources that allow Kubernetes to understand and allocate ComputeDomains. This enables advanced topology management for distributed AI workloads on EC2 P6e-GB200 UltraServers.

### Step 6: Verify DRA Resources

Check if the DRA resources are available:

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

## Validate IMEX Channels

Once the GPU Operator and DRA driver are configured, you can create IMEX channels to enable direct GPU memory access across nodes. The example below shows how a ComputeDomain resource automatically provisions the required IMEX infrastructure:

Create `imex-channel-injection.yaml`:

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

This YAML creates a ComputeDomain resource and references it from a pod. The ComputeDomain controller automatically generates the ResourceClaimTemplate, which the pod uses to access the IMEX channel. Behind the scenes, this triggers deployment of the IMEX daemon on the selected node and dynamically establishes the IMEX domain instead of requiring a static setup.

**Apply and verify:**

You should see the pod `imex-channel-injection-...` in Running state.

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
# [INFO] IMEX version 570.133.20 is running with configuration options
# [INFO] GPU event successfully subscribed
# [INFO] Connection established to node 0 with IP 192.168.56.245
```

The logs show IMEX initialization, gRPC setup between nodes, and confirmation that the unified memory domain is active. GPUs across nodes in the UltraServer can now access memory directly over NVLink, providing unprecedented performance for distributed AI workloads.

---

## Multi-Node IMEX Communication in Practice

To illustrate how the DRA driver coordinates GPU communication across nodes, the next section deploys a multi-node MPI benchmark using IMEX channels for high-bandwidth GPU-to-GPU memory transfers across EC2 P6e-GB200 UltraServer nodes.

### Deploy Multi-Node MPI Job

Create `nvbandwidth-test-job.yaml`:

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

Apply with:

```bash
kubectl apply -f nvbandwidth-test-job.yaml
```

1. **ComputeDomain creation and node selection:** The DRA driver immediately selects nodes:

   * Identifies 2 nodes with available GB200 GPUs
   * Ensures nodes belong to the same NVLink domain
   * Creates the ComputeDomain resource

2. **IMEX domain establishment:** DRA automatically:

   * Deploys IMEX daemons on both nodes
   * Configures cross-node gRPC channels
   * Sets up shared memory mappings between GPUs

The experiment shows that DRA transforms multi-node GPU clusters into a unified resource, enabling LLM training across UltraServer nodes with native GPU memory access while maintaining optimal performance. All 72 GPUs in a u-p6e-gb200x72 UltraServer appear as a single memory domain, with Kubernetes orchestrating IMEX automatically so data teams can focus on modeling instead of infrastructure.


## Conclusion

Amazon EC2 P6e-GB200 UltraServers on Amazon EKS represent a major advancement for users looking to train and deploy trillion-parameter AI models at scale. The combination of NVIDIA Grace Blackwell GPUs with NVLink, support from Amazon EKS, DRA, and NVIDIA tooling has made exascale AI computing accessible through familiar container management patterns.

The integration of IMEX channels and NVLink enables a unified GPU memory cluster across nodes and racks, breaking the traditional limits of node-local GPU computing. This architectural improvement unlocks new possibilities for:

* Training foundation models with trillions of parameters
* Running multimodal AI workloads with real-time performance requirements
* Deploying complex inference pipelines with sub-second latency

To get started with DRA on Amazon EKS, refer to the **Amazon EKS AI/ML** documentation for comprehensive guidance and explore the **AI on EKS** project, which provides **DRA** examples you can experiment with and deploy in your environment.

{{% notice warning %}}
**SECURITY NOTE:** The configurations presented in this article are basic examples intended to illustrate core functionality. In production environments, you should implement additional security controls. Contact your AWS account team for guidance on using P6e-GB200 on Amazon EKS.
{{% /notice %}}

---

## About the Authors

![Vara Bonthu](/images/Blog/Blog_2/blavt2_1.png)

**Vara Bonthu** is a Senior SA specializing in open-source data and AI on EKS at AWS, driving open-source initiatives and supporting customers across organizations. He has deep expertise in open-source technologies, data analytics, AI/ML, and Kubernetes, with extensive experience in development, DevOps, and architecture.

---

![Chris Splinter](/images/Blog/Blog_2/blavt2_2.png)

**Chris Splinter** is a Senior Product Manager on the Amazon EKS team, focusing on helping customers run AI workloads with Kubernetes.

---

![Nick Baker](/images/Blog/Blog_2/blavt2_3.png)

**Nick Baker** is a Software Development Engineer on the Node Runtime team at Amazon EKS. He focuses on enhancing support for accelerated workloads and improving data-plane stability on EKS.
