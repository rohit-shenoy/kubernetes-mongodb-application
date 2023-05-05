# kubernetes-poc
Kubernetes PoC

Master - K8 Control Plane node that handles global cluster configurations/tasks
Consists of:
    a) API Server: Frontend for K8 API to UI, CLI (kubectl) & API. We interact with the K8 cluster using the API Server.
    b) Controller Manager: Controls cluster configuration using controller processes, handles node failure etc.
    c) Scheduler: In charge of placing new application pods smartly on nodes
    d) etcd - HA k/V store for storing cluster state (nodes & container states - for backup & recovery)
              Holds at any time the current status of any K8s component

Node - Worker machines that run application containers (in Pods)

Pods - Smallest unit of K8, Abstraction layer over container, so that only K8 components used for Interaction, Usually 1 Application per Pod

Service - Pods communicate with each other using a service, An IP that gets assigned to every Pod for communication (internal or external), but is re-created upon
          Pod re-creation (pods are ephemeral). So service assigns a permanent, static IP to each Pod (with DNS name), lifecycle of pod/service unconnected. Service is also a Load Balancer (when multiple pods exist)

Ingress -  External Service allows communication from browser (outside), but Service assigns a node-ip:port URL for service, so Ingress is used.
          Routes traffic into cluster, requests goes to Ingress, and it resolves it to the service (hosting application of https:my-app.com/port) URL

ConfigMap -  Saves external configuration for your application (URLs of other services...) so that you don't have to rebuild container images & redeploy Pods for any configuration changes. But it's for non-confidential data only (not credentials)

Secret - Stores secret data (credentials, keys, certificates) in encrypted format (base64 encoded). Pods use it like Config map, by referencing it in Deployment/Pod, and application can use it as environment variables, properties file.

Volume - Physical storage on hardrive that gets attached to a Pod to persist it's data permanently (so that data is not lost if pod dies). Storage can be local machine, remote outside of K8 cluster (cloud or external on-prem). Kubernetes cluster itself doesn't manage any data persistence. 

Deployment - Blueprint for "your application" pods, with replicas specified. In practice, you create Deployments, and scale them up/down using replicas. Deployment is an abstraction over Pods. For Stateless Applications.
 
StatefulSet - Databases or applications that save state (data) can't be replicated using Deployments, as they save data. To avoid data inconsistencies, multiple Pods that save data (say app DB pods), need to share storage, and manage writes to avoid inconsistencies. For STATEFUL apps (applications like DBs - MySQL, MongoDB, ElasticSearch). Like Deployment, handles replicas, scaling up/down but also manages synchronization for data consistency.

Kubernetes Configuration:
- kubectl (CLI) used to interact with the API Server to request cluster configuration changes (deploying Pods, other components etc)
- YAML or JSON format
- Declarative format (we declare what we need)
- K8 uses controller-manager to check IS == should states (desired state == actual state)

Each configuration file has 3 Parts:
~ apiVersion & kind specify what type of resource
 1. metadata (names, labels)
 2. specification (attributes of your resource - specific to kind of resource being created
 3. status (auto generated and added by k8) - used by K8 to check desired state == actual state (self-healing)



### install virtualbox and minikube
`brew update`

`brew install hyperkit`

`brew install minikube`

`kubectl`

`minikube`

### create minikube cluster
`minikube start`

`kubectl get nodes`

`minikube status`

`kubectl version`

### delete cluster and restart in debug mode
`minikube delete`

`minikube start --v=7 --alsologtostderr`

`minikube status`

### kubectl commands
`kubectl get nodes`

`kubectl get pod`

`kubectl get services`

`kubectl create deployment nginx-depl --image=nginx`

`kubectl get deployment`

`kubectl get replicaset`

`kubectl edit deployment nginx-depl`

### debugging
`kubectl logs {pod-name}`

`kubectl exec -it {pod-name} -- bin/bash`

### create mongo deployment
`kubectl create deployment mongo-depl --image=mongo`

`kubectl logs mongo-depl-{pod-name}`

`kubectl describe pod mongo-depl-{pod-name}`

### delete deplyoment
`kubectl delete deployment mongo-depl`

`kubectl delete deployment nginx-depl`

### create or edit config file
`vim nginx-deployment.yaml`

`kubectl apply -f nginx-deployment.yaml`

`kubectl get pod`

`kubectl get deployment`

### delete with config
`kubectl delete -f nginx-deployment.yaml`

#Metrics

`kubectl top` The kubectl top command returns current CPU and memory usage for a cluster’s pods or nodes, or for a particular pod or node if specified.




 