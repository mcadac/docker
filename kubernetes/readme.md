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
            name:
            namespace:
            labels: 
        spec: {Dictionary to put the containers for deploying in the pod - This defines what inside the object we are creating}
            containers:
                -   name: {custom name}
                    image: {image name in the docker hub}
        ```
    -

## Minikube Commands
       
        ```brew cask install minikube```
        
        To start cluster
        ```minikube start```
        
        To delete cluster
        ```minikube delete```
        
        To log into the Minikube
        ```minikube ssh``

## Kubectl commands

    ```
    kubectl cluster-info
    kubectl describe pods
    kubectl describe {kind}
    kubectl run nginx --image nginx
    kubectl delete {kind} {name}
    kubectl create -f pod-definition.yml 
    kubectl create -f pod-definition.yml --namespace=dev
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
    kubectl create configmap <name> --from-literal=<key>=<value>
    kubectl create configmap <name> --from-file=<path>
    kubectl create -f test-config.yml
    kubectl get configmaps
    kubectl describe configmaps
    kubectl get secret app-secret -o yaml
    kubectl describe secrets
    kubectl describe pod {pod-name}
    kubectl exec -it {pod-name} ls /var/run/secrets/kubernetes.io/serviceaccount

    kubectl create serviceaccount dashboard-sa  (It creates a token) -> (saved in a secret object)
    kubectl get serviceaccount
    kubectl describe serviceaccount dashboard-sa
    kubectl describe secret dashboard-sa-token-{value}
         curl https://192.168.56.70:6443/api -insecure --header "Authorization: Bearer {token}"


    -- Extract the definition file to a file
    kubectl get pod <pod-name> -o yaml > pod-defnition.yaml
    kubectl edit pod <pod-name>
    kubectl get pods --namespace=kube-system
    kubectl get pods --all-namespaces
    kubectl create namespace dev
    kubectl config set-context $(kubectl config current-context) --namespace=dev
    

    Creation commands:

        -- Create an NGINX Pod
        kubectl run --generator=run-pod/v1 nginx --image=nginx

        -- Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
        kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml

        -- Create a deployment
        kubectl run --generator=deployment/v1beta1 nginx --image=nginx

        -- Or the newer recommended way:
        kubectl create deployment --image=nginx nginx

        -- Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
        kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run -o yaml
        kubectl create deployment --image=nginx nginx --dry-run -o yaml

        -- Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)
        kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml
        
        -- Save it to a file - (If you need to modify or add some other details)
        kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml > nginx-deployment.yaml

        -- Create a Service named nginx of type NodePort and expose it on port 30080 on the nodes:
        kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run -o yaml

    How to use local images:

        # Start minikube
        minikube start

        # Set docker env
        eval $(minikube docker-env)

        # Build image
        docker build -t foo:0.0.1 .

        # Run in minikube
        kubectl run hello-foo --image=foo:0.0.1 --image-pull-policy=Never

        # Check that it's running
        kubectl get pods

    ```

## Docker 
    - Container are specific task or process, they live as long the process alive
    - Dockerfile 
        -> CMD command to define which program will be run
        -> ENTRYPOINT command to define first command for running 
            e.g: ENTRYPOINT ["sleep"] -> docker run ubuntu-sleeper 10 -> it'll be use the value 10 

    1. Commands:
        - docker run ubuntu [commands] e.g docker run ubuntu sleep 5
        - docker run --entrypoint sleep2.0 ubuntu-sleeper 10
        




## Advance
    1. Namespace: Place where all objects of kubernets will be created

        - Kubernetes namespace:

            - kube-system: 
                Kubernetes creates a set of pods and service for internal purpose such as netwoking DNS in order to isolate these from the user (avoid delete or modifications by the user)  

            - Default: 
                Main namespace

            - kube-public:
                Contains resources that should be available to all users are created

        - To connect:
            
            - To use a service in other namespace we should use the next name parameters because the name is created like this in the DNS service.
                serviceName.namspace.svc.cluster.local

        - To create a new namespace
            
            - definition file

                apiVersion: v1
                kind: Namespace
                metadata: 
                    name: (newName)

            - command: kubectl create namespace dev

        - To define the main namespace in my system

            - kubectl config set-context $(kubectl config current-context) --namespace=dev

        - To define resources for the namespace

            - Resource quota
                - deinition file:

                    apiVersion: v1
                    kind: ResourceQuota
                    metadata:
                        name: computer-quota
                        namespace: dev
                    spec:
                        hard:
                            pods: "10"
                            requests.cpu: "4"
                            requests.memory: 5Gi
                            limits.cpu: "10"
                            limits.memory: 10Gi

                - kubectl create -f quota.yml



## References and web pages
    - https://www.osboxes.org
