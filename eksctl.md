https://eksctl.io/usage/windows-worker-nodes/#
https://docs.aws.amazon.com/eks/latest/userguide/sample-deployment.html

Creating a new Windows cluster¶
The config file syntax allows creating a fully-functioning Windows cluster in a single command:

```
# cluster.yaml
# An example of ClusterConfig containing Windows and Linux node groups to support Windows workloads
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: windows-cluster
  region: us-west-2

nodeGroups:
  - name: windows-ng
    amiFamily: WindowsServer2019FullContainer
    minSize: 2
    maxSize: 3

managedNodeGroups:
  - name: linux-ng
    instanceType: t2.large
    minSize: 2
    maxSize: 3

  - name: windows-managed-ng
    amiFamily: WindowsServer2019FullContainer
    minSize: 2
    maxSize: 3
```

```
eksctl create cluster -f cluster.yaml
```

To ensure workloads are scheduled on the right OS, they must have a nodeSelector targeting the OS it must run on:

```
# Targeting Windows
  nodeSelector:
    kubernetes.io/os: windows
    kubernetes.io/arch: amd64
```
```
# Targeting Linux
  nodeSelector:
    kubernetes.io/os: linux
    kubernetes.io/arch: amd64
```
