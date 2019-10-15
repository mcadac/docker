# Kubernetes or K8s
    - The OS kernel is responsible for interacting with the underlying hadware 
    - Docker image: Package or template used to create container    
    - Kubernetes is a container Orchestration technology

## Container
    - They are completely isolated env.
    - They can have their own process like virtual machines.
    - They have less isolation.
 
 ## Node (Minios)
    - It is a machine (Physical or vitual)
    - It is where Kubernetes is installedx
    - Master node: 
        - It watches over the nodes (worker nodes) in the cluster and is responsible for the Orchestration

## Components of Kubernetes
    - API server: It as the front -> Management
    - Scheduler: It is responsible for distribuing work or containers across multiple nodes.
    - Controller: They are responsible of the Orchestration and makes decisions to bring up an container
    - Container runtime: It is underlying software for running a container
    - Kubelet: It is an agent that runs on each node in the cluster.
    - kubectl: Tool to deploy and manage applications on a Kubernetes cluster (command line utility used to manage a kubernetes cluster)
    - etcd : key-value store, this component stores all information about the nodes and is responsible for implementing locks within the cluster to avoid conflicts between the masters

## Kubernetes objects

    - Pods (Kubernetes objects): 
        Basic execution unit, represents an execution unit running on the cluster
        Encapsulates one or more containers, storage resources
        
        One container per Pod: Most common option
        Multiple containers per Pod: Encapsulate multiple co-located

    - Replication controller (Deprecated)
        
        Help high availability... It can deploy Pods automatically when someone is down..
        In the replication file, you define the Pod configuration (metadata to spec) in the spec section.
    
    - Replication set (New)
        The replicaset works with labels.
        If we create a new pod with differ label, the replicaset will not filter the new pod.

    - Deployment startegy
        

    - ReplicaSet:
        Maintains a stable set of replicas Pods running at any

    - Deployments:

        - Capabilities: Deploy, upgrade, Rolling updates, Rollback, Pause Environment
        - Order of the objects:  Deployment -> ReplicaSet -> Pods
        - One of the most common workloads to directly create o manage
            1. Recreate (App down)
            2. Rolling update
            3. Allowing to create pods, replica set

        - Rollout and rollback
            1. When you create a deployment it triggers a rollout called REVISION 1
            2. When the container version is updated a new REVISION 2 is created. (It keep both REVISIONS)
        - Deployment startegy
            1. Recreate: The old Application is down and after that tha new apllication version will be deployed.
            2. Rolling update (Default): The applications are updating one by one
    
    - Stateful set:
        Deployment order
        Data persisint
        Stable networkin

    - Daemon set:
        Run a copy of a pod
    
    - Jobs and cron Jobs    
        task-based workflow
        Useful if you need to perfom one-off

    - Service
        - A service enable communication between varios components within and outside of the application.
        - Helps us connect applications together with other applications or users.
        - Liston on a port in the node and forward request to the node
        - Types:
            - NodePort: Node port -> Service port -> Target port (Pod)
            - ClusterIp: It is useful when we are creating a group of pods, therefore a ClusterIp is an only point of access to the grup.
            - LoadBalancer: 

        - The metadata -> name property is so important because it is the name which other service or Kubernetes object can use to find it.
        - To expose service to the outside of the cluster

    - Volumes and persintent volumes
    - Labels and annotations
        Tag can be attached to Kubernetes objects to mark them
        These help the management and routing


    - Networking
        1. Network inside the node
        2. Each pod have an IP
        3. Comunication between nodes using localhost
    

## Tools to set up kubernetes
    - Minikube:
        - It set up just one an instance 
    - KubeAdmin:
        - It is used to set up a multi node

## Kubernetes files
    - All files of kubernetes have the next fields:
        ```
        apiVersion: {kubernetes api version}
        kind: {type of the object}
        metadata: {All attributes for the specific object}
        spec: {Dictionary to put the containers for deploying in the pod - This defines what inside the object we are creating}
            containers:
                -   name: {custom name}
                    image: {image name in the docker hub}
        ```
    -

## Minikube
    Commands
       
        ```brew cask install minikube```
        
        To start cluster
        ```minikube start```
        
        To delete cluster
        ```minikube delete```
        
        To log into the Minikube
        ```minikube ssh``

## Kubectl

    ```
    kubectl cluster-info
    kubectl describe pods
    kubectl describe {kind}
    kubectl run nginx --image nginx
    kubectl delete {kind} {name}
    kubectl create -f pod-definition.yml
    kubectl get pods
    kubectl get pods -o wide
    kubectl get replicationcontroller
    kubectl get replicaset 
    kubectl replace -f replicaset-definition.yml
    kubectl scale --replicas=6 -f replicaset-definition.yml
    kubectl scale --replicas=6 replicaset myapp-replicaset
    kubectl get all
    kubectl create -f deploymeny-definition.yml --record
    kubectl rollout status deployment/{deployment-name}
    kubectl rollout history deployment/{deployment-name}
    kubectl rollout undo deployment/{deployment-name}
    kubectl apply -f {file-name}
    kubectl get services
    ```

## References and web pages
    - https://www.osboxes.org
