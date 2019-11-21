# Expo

    Kubernetes Architecture:

        Master code:
            Responsable for managing the cluster
            information memeber of the cluster
            nodes monitored
            It does the work when there is a node with a fail

            Api-server:
                - allow applications to communicate with one another 
                -  The users, management devices, and command line interfaces all talk to the API server to interact with the Kubernetes cluster.
            
            Scheduler:
                - It is responsible for distribuing work or containers across multiple nodes.

            Controller manager:
                - They are responsible of the Orchestration and makes decisions to bring up an container
        
            ETCD:
                - key-value store, this component stores all information about the nodes and is responsible for implementing locks within the cluster to avoid conflicts between the masters

        POD Minions:
            smallest deployable unit 

            Kubelet: 
                - It is an agent that runs on each node in the cluster.
            kubectl: 
                - Tool to deploy and manage applications on a Kubernetes cluster (command line utility used to manage a kubernetes cluster)
        

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
            Useful if you need to perfom one-off (Exceptional)

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



    Namespace:
        Each aspect of a container runs in a separate namespace and its access is limited to that namespace

    Groups:
        more importantly, that a single container cannot bring the system down by exhausting one of those resources.

    Kubernetes security:
        Capabilities are only supported at the container level

   Accounts:
    User account:

        Humans
        e.g admin task

    Service account:

        machines -> require authentication -> you can assign permissions
        e.g prometheus -> to pull from kubernetes-api performance information 
        e.g Jenkins -> to deploy apps
        Each namespace have its own service account -> It is used in every pod -> it is created such as mount inside the POD.
            e.g kubectl describe pod {pod-name}
            e.g kubectl exec -it {pod-name} ls /var/run/secrets/kubernetes.io/serviceaccount
            kubectl create serviceaccount dashboard-sa  (It creates a token) -> (saved in a secret object)

        kubectl create serviceaccount dashboard-sa  (It creates a token) -> (saved in a secret object)
        kubectl get serviceaccount
        kubectl describe serviceaccount dashboard-sa
        kubectl describe secret dashboard-sa-token-{value}
        curl https://192.168.56.70:6443/api -insecure --header "Authorization: Bearer {token}"





# Practice: 

    ## Commands and arguments 
        ````
            FROM ubuntu
            ENTRYPOINT [ "echo" ]
            CMD [ "Default log mates!" ]    

            FROM ubuntu
            ENTRYPOINT ["top", "-b"]
            CMD ["-c"]
        ```

        ````
            apiVersion: v1
            kind: Pod
            metadata:
            name: ubuntu-logger-pod
            spec:
            containers:
                - name: ubuntu-logger
                image: ubuntu-logger
                imagePullPolicy: Never   
                args: ["other log"]
        ``` 

        ````
            minikube start
            eval $(minikube docker-env)
            docker build -t foo:0.0.1 .
            --kubectl run hello-foo --image=foo:0.0.1 --image-pull-policy=Never
            kubectl create -f ubuntu-logger-pod
            kubectl get pods
        ```

    ## Environment Variable

        1. ConfigMaps

            ````
                apiVersion: v1
                kind: ConfigMap
                metadata:
                name: myconfigmap
                labels:
                    app: myapplication
                data:
                POL_HOME: /usr/local/payu
                ENV: DEV
            ```
            kubectl create -f test-config.yml
            kubectl get configmaps
            kubectl describe configmaps

            create Dockerfile
            create pod
            kubectl create -f ....
            kubectl logs pod-name