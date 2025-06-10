# DevOps-Learning-Kubernetes
Repository containing commands and topics I learnt in the CoderCo Kubernetes Module.

## What is K8s and why we use it?
- Kubernetes is container orchestration tool that assists in deployment, scaling and management. 
- Large open-source project

## K8s Architecture
- Cluster: a collection of nodes that provide compute, memory, storage and networking resources. Kubernetes uses these resources to run the various workloads. Your infrastructure may consist of multiple clusters.
- Master Node: The brain of the operation. Controls everything in the cluster. Consists of:
    - Kube API Server: The entrypoint for all administrative commands. When you tell kubernetes to deploy applications, the API server is the component that makes it happen.
    - ETCD: Like the memory. Stores all the cluster data and the state. If anything goes down, Kubernetes checks etcd for what is supposed to be running and where.
    - Kube Controller Manager: The cluster supervisor, ensuring the desired state of the cluster matches reality. If something goes wrong, it steos in to correct it.
    - Kube Scheduler: The match maker of the cluster. It decides which worker node your new pod will be running on based on resource availability. 
    - Cloud Controller Manager: If you are using a cloud based kubernetes provider, then you will have this. Connects your cluster to your cloud provided API to handle things like load balancing, storage, networking and so much more. 
- Worker Nodes: The hands on workers of the cluster. They actually run your applications. These consist of:
    - Kubelet: An agent that talks to the master node, making sure containers are running as they should be. If a pod needs to be spun up, the kubelet handles that.
    - Kube-proxy: Handles the networking side of things. Ensures that each pod in the node can communicate with other pods in the cluster no matter where they are running. 
    - Pod: The smallest unit in kubernetes. They are like wrappers for containers. A pod can contain one or more containers.