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

## Kubernetes in Docker (kind)
- To create a cluster, create a yaml file which contains the below
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes: 
- role: control-plane
- role: worker
- role: worker

```

- To deploy the cluster, run the following command 
```
kind create cluster --config name-of-file.yaml --name cluster-name
```

- To view the cluster run the command
```
kubectl cluster-info --context kind-name-of-cluster
```

- You can check the nodes by running `kubectl get nodes`

- Once you have the nodes set up, you can run pods on these nodes by running the command:
```
kubectl run pod name_of_node --image=container_image
```

- You can check the pods by running `kubectl get pods`

- To create a deployment, run the command:

```
kubectl run deployment name_of_node --image=container_image
```

- You can check the deployments by running `kubectl get deployments`

- To delete the cluster, you run the command:

```
kind delete cluster --name name_of_cluster
```

## Pods

A pod is the smallest unit of work or resource found in kubernetes. Each pod contains one or more containers. Containers in a pod are always scheduled together (run on the same machine). All containers in a pod have the same IP address and port space, meaning they can communicate using a local host. All the containers in a pod have access to a shared local storage on the node that is hosted in the pod. Containers do not get access by default to local storage, volumes must be explicitly mounted to each container on the pod. Pods can be defined in a definition file as can be seen below;

```
apiVersion: v1
Kind: Pod
Metadata:
    labels:
        run: nginx
    name: nginx
Spec:
    containers:
    - image: nginx
      name: nginx

```

To run the definition file, we run the command

```
kubectl apply -f name_of_file.yaml
```

If we create a pod imperitively (through the command line), we can view the actual yaml file created by the command by running the command `kubectl get pod pod_name -o yaml`.

## Deployments
For large scale operations where you need many instances, instead of initialising pods one by one, you would use a deployment. A deployment is a powerful kubernetes controller that lets you manage your application across multiple instances effortlessly.Acts like a project manager. It ensures the right number of pods are always running as specified. You can scale up or down depending on how many pods you need. You can easily pgrade or roll back deployments. The deployment definition file will look like the below;

```
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-deployment
    labels:
        app: nginx
spec:
    replicas: 3
    selector:
        matchLabels:
            app: nginx
    template:
        metadata:
            labels:
                app: nginx
        spec:
            containers:
            - name: nginx
              image: nginx
            ports:
            - containerPort: 80
```

To run the definition file, we run the command

```
kubectl apply -f name_of_file.yaml
```

To create a deployment imperitively, you run the command:

`kubectl create deployment name_of_deployment --image=image_name --replicas=2`

## ReplicaSets
The simplest pod controller is the replicaSet. Ensures ensures a speciffied number of pod instances are running at any given time. If one pod fails, the replicaSet spins up a new one to replace it. Deployments are better as it creates a pod manager behind the scenes as well as comes with features such as automated roll backs and rolling updates. The deployment handles all the hardwork for you. 