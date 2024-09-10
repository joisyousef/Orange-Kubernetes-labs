```
# Kubernetes Resources: Limits and Requests

This project contains YAML configuration examples to demonstrate how to set **limits** and **requests** for containers in a Kubernetes environment. These configurations help manage the resource consumption (CPU and memory) of containers, ensuring efficient resource allocation and preventing resource overconsumption.

## Table of Contents

- [Introduction](#introduction)
- [What Are Limits and Requests?](#what-are-limits-and-requests)
  - [Requests](#requests)
  - [Limits](#limits)
- [Example YAML Configuration](#example-yaml-configuration)
- [How to Apply the YAML File](#how-to-apply-the-yaml-file)
- [Best Practices](#best-practices)
- [References](#references)

## Introduction

In Kubernetes, **requests** and **limits** are used to manage the resources assigned to containers within a pod. This ensures that containers have enough resources to operate effectively and also prevents them from consuming too many resources, which could affect other containers running on the same node.

## What Are Limits and Requests?

### Requests

- **Definition**: The minimum amount of resources that a container is guaranteed.
- **Purpose**: Ensures that a container gets the requested resources. The pod will only be scheduled on a node that can meet this request.
- **Impact**: If resources are not available, the pod will not be scheduled. However, a container can use more than its request if resources are available, up to its limit.

### Limits

- **Definition**: The maximum amount of resources a container can use.
- **Purpose**: Prevents a container from consuming too many resources.
- **Impact**: If the container exceeds its limit, Kubernetes may throttle its CPU usage or terminate the container if memory exceeds the limit.

## Example YAML Configuration

Here is an example YAML configuration file (`example-pod.yaml`) showing how to set limits and requests for a pod running the `nginx` container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

### Explanation:
- **Requests**:
  - `memory: "64Mi"`: The container is guaranteed at least 64 MiB of memory.
  - `cpu: "250m"`: The container is guaranteed 250 milliCPUs (25% of a CPU).
  
- **Limits**:
  - `memory: "128Mi"`: The container is restricted to using a maximum of 128 MiB of memory.
  - `cpu: "500m"`: The container is limited to 500 milliCPUs (50% of a CPU).

## How to Apply the YAML File

To apply the `example-pod.yaml` file in your Kubernetes cluster, use the following command:

```bash
kubectl apply -f example-pod.yaml
```

Once applied, Kubernetes will schedule the pod according to the resource requests and enforce the limits during runtime.

To verify the pod status:

```bash
kubectl get pod webapp
```

To check resource allocation and consumption:

```bash
kubectl describe pod webapp
```

## Best Practices

- **Set requests for critical pods**: Ensure essential applications have enough resources by defining requests.
- **Set limits to avoid overconsumption**: Limit resources to prevent containers from consuming all available node resources.
- **Monitor resource usage**: Regularly review and adjust resource configurations based on actual usage.
- **Use resource quotas**: Apply resource quotas for namespaces to ensure balanced resource distribution across services.

## References

- [Kubernetes Resource Management Documentation](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
- [Best Practices for Resource Limits](https://kubernetes.io/docs/concepts/policy/limit-range/)

---

This project serves as a guide for understanding how to manage Kubernetes resources with limits and requests. Feel free to contribute or open issues for discussions!

```
