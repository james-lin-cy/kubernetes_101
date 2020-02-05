# Pre-setup

A Kubernetes cluster, if you don't have one, please install minikube in you machine https://minikube.sigs.k8s.io/docs/start/macos/
```
brew install minikube
brew install hyperkit
# minikube config set vm-driver hyperkit
minikube start --vm-driver=hyperkit
```

Example files
`git clone git@github.com:james-lin-cy/kubernetes_101.git`

# Agenda 

- Virtual machine, Docker and Kubernetes
- Operation
  - Basic operation
  - Advanced operation
- Container, Pod, Deployment, Service, Configmap


# Virtual machine, Docker and Kubernetes

## From Dedicated server to Virtual machine

Single server(DELL R730) → Hypervisor(ESXi 6.5)

Why
- Too many Computing power 
- Isolate environment
- HA and backup

## From Virtual machine to Docker

Hypervisor(ESXi, Virtualbox) → Docker

Why
- Resource
- Easy

### The Cluster
Vagrant → Docker compose
Vagrant: base on VirtualBox
Docker compose: base on Docker
They are in a single machine and most of them are local

## From Docker to Kubernetes

Why
- Kubernetes (A.K.A. k8s) is an open-source container-orchestration system for automating application deployment, scaling, and management


# Operation

## Basic Operation

### Get resource

```
kubectl get ${resource name}
kubectl get po [pod-name]# kubectl get pod
kubectl get deploy [deployment-name] # kubectl get deployment
kubectl get svc [service-name] # kubectl get service
kubectl get cm [configmap-name] # kubectl get configmap
```

### Describe resource

```
kubectl describe ${resource name}
kubectl describe po [pod-name] # kubectl get pod
kubectl describe deploy [deployment-name] # kubectl get deployment
kubectl describe svc [service-name] # kubectl get service
kubectl describe cm [configmap-name] # kubectl get configmap
```

### Show logs

```
kubectl logs [pod-name] -c [container-name]
```

### Get into pod

```
kubectl exec -ti [pod-name] -c [container-name] -- ${command}
```

### Apply/Create/Replace/Delete object

```
kubectl apply -f <filename|url>
kubectl create -f <filename|url>
kubectl replace -f <filename|url>
kubectl delete -f <filename|url>
```

Q: What's different between apply and create?

A: kubectl create is what we call Imperative Management. On this approach you tell the Kubernetes API what you want to create, replace or delete, not how you want your K8s cluster world to look like. kubectl apply is part of the Declarative Management approach, where changes that you may have applied to a live object (i.e. through scale) are maintained even if you apply other changes to the object.

## Advanced operation

### Edit resource

```
kubectl edit ${resource name} ${name}
```

### Scale Replica

```
kubectl scale --replicas=${replica} <deploy/sts>/${name}
```

### Append

- Dryrun
- Different namespaces
- patch

# Container, Pod, Deployment, Service, Configmap

## Container

In Docker, the basic running element is container

In Kubernetes, containers are inside the Pod

## Pod

In Kubernetes, the basic running element is pod

And containers are inside the pod

```
kubectl get pod
kubectl apply -f pod-centos.yaml
kubectl get pod
kubectl logs centos-cli
kubectl exec -ti centos-cli -- sh
# minikube kubectl -- exec -ti centos-cli -- sh
```

Q: Why can we see container logs in kubernetes?

A: It will show all messages direct to stdout/stderr 

## Deployment

In most of time, we describe a desired state in a Deployment and then deploy it

```
kubectl apply -f deployment-echoserver.yaml
kubectl get deployment
kubectl get pod
kubectl exec -ti echoserver-deployment-77c4597b88-rlxtm -- curl -k https://localhost:8443
```

There are similar controller like Deployment
- DaemonSet
- StatefulSet
- Job
- CronJob

## Service

If you want to let busybox to access echoserver, you have to expose the echoserver to create a service

```
kubectl apply -f service-echoserver.yaml
kubectl get service
kubectl exec -ti centos-cli -- curl -k https://service-echoserver
```

## ConfigMap

ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable

```
kubectl create configmap nginx-index --from-file=index.html
kubectl apply -f configmap-nginx.yaml
kubectl get pod
kubectl exec -ti nginx-deployment-5bb89cfddb-44l9q -c centos -- curl localhost
kubectl delete configmap nginx-index
kubectl create configmap nginx-index --from-file=index.html
# kubectl annotate pod --overwrite nginx-deployment-5bb89cfddb-44l9 update="$(date)"
kubectl create configmap nginx-index --from-file=index.html
kubectl exec -ti nginx-deployment-5bb89cfddb-44l9q -c centos -- curl localhost
```

## Append

- StatefulSet, DaemonSet, Job, Cron job
- secret
- ServiceAccount
- Role, RoleBinding
- ClusterRole, ClusterRoleBinding
- Service Type
  - ClusterIP, NodePort, LoadBalancer


# Kubernetes beyond 101
- Kubernetes master/worker nodes
- Kubernetes networking (firewall)
- How to develop/test in a python container
- How to build docker image
- How to troubleshooting pods
- How to write k8s deployment YAML