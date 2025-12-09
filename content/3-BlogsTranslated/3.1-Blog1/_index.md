---

title: "Dynamic Kubernetes Request Right Sizing with Kubecost"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
----------------------

# Dynamic Kubernetes Request Right Sizing with Kubecost

This article explores how to use the **Kubecost Amazon Elastic Kubernetes Service (Amazon EKS) add-on** to reduce infrastructure costs and optimize Kubernetes performance. The *Container Request Right Sizing* feature helps assess how container requests are configured, identify inefficiencies, and optimize them manually or automatically.

We will also cover how to evaluate right sizing recommendations and apply updates either once or on a schedule, helping your Amazon EKS environment stay continuously optimized.

## What is a Container Request?

In Kubernetes, a *container request* is the minimum CPU and memory a workload declares so the scheduler can place the pod on an appropriate node.

* The scheduler finds nodes with sufficient unused resources.
* Once a pod is placed, those resources are “reserved” even if the container doesn’t fully use them.
* Overly high requests → wasted resources and higher costs.
* Requests also affect the Quality-of-Service (QoS) tier and eviction decisions.

## Kubecost Savings Insights

The Kubecost Amazon EKS add-on provides detailed visibility into containers that request excessive resources.

The *Container Request Sizing* dashboard shows:

* Containers that can be optimized
* Current CPU/memory request efficiency
* Average and peak CPU/memory usage
* Estimated monthly cost savings
* Total potential cluster savings

![1](/images/Blog/Blog_1/bl1_1.png)

> *Figure 1: Container Request Right Sizing recommendations*

Right sizing is especially effective in dev, test, and staging environments where performance requests are often “over-provisioned”.

## Customizing Resize Recommendations

Kubecost allows adjusting recommendations based on:

* Workload type
* Criticality
* Operational goals

You can choose prebuilt profiles such as:

* `development`
* `production`
* `high availability`

Or create your own custom profile.

Adjustable parameters include:

* CPU/memory target utilization
* Query window (48h, 7 days, …)
* Filter workloads by label, namespace, controller

## Acting on Kubecost Recommendations

### 1. One-time Resize

**Resize Requests Now** in the dashboard applies the suggested requests immediately to:

* Deployments
* StatefulSets
* Jobs

Reflecting the profile you selected.
![2](/images/Blog/Blog_1/bl1_2.png)

> *Figure 2: Enable Resize Requests and Enable Autoscaling*

### 2. Scheduled Right Sizing

Select **Enable Autoscaling** to:

* Set up recurring resize jobs
* Update requests based on recent usage data
* Maintain optimized requests over time

Example:
A job runs every 2 hours, based on 48h of data, targeting 80% CPU/memory utilization.

### 3. Automation via Helm

You can enable automatic right sizing during Kubecost installation with Helm.

Example configuration:

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
```

This configuration:

* Runs every 2 hours
* Targets 80% utilization
* Applies to all Deployments
* Ideal for platform teams wanting to enforce best practices automatically

## Conclusion

Practical benefits from Kubecost request sizing:

* Reduce **20–60%** of compute costs in non-production environments
* Increase node utilization
* Faster pod scheduling
* Optimize performance and reduce bottlenecks

Kubecost helps you:

* Identify inefficient workloads
* Get data-driven recommendations
* Customize optimization strategies
* Apply changes manually or automatically

If you haven’t used Kubecost yet, start at the Get Started page to install it in your EKS cluster.

## About the Authors

| ![Kai](/images/Blog/Blog_1/blavt1.png) | **Kai Wombacher**
Product Director at IBM Kubecost, specializing in optimizing large-scale Kubernetes costs. |

| ![Jason](images/Blog/Blog_1/blavt2.png) | **Jason Janiak**
Partner Solutions Architect at AWS. |

| ![Mike](images/Blog/Blog_1/blavt3.png) | **Mike Stefaniak**
Senior Director, Amazon EKS Product Team at AWS. |
