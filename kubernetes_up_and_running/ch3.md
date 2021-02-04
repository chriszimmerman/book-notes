# Chapter 3 - Deploying a Kubernetes Cluster

## Installing Kubernetes on a Public Cloud Provider

### GCloud

Set default zone:

`gcloud config set compute/zone us-west1-a`

Create cluster:

`gcloud container clusters create kuar-cluster`

Get cluster credentials:

`gcloud auth application-default login`

## Installing Kubernetes Locally Using minikube

`minikube start`

`minikube stop`

`minikube delete`

## Running Kubernetes in Docker

Create cluster:

```
kind create cluster --wait 5m \

export KUBECONFIG="$(kind get kubeconfig-path)"

kubectl cluster-info

kind delete cluster
```

Get cluster diagnostic:

`kubectl get componentstatuses`

List nodes:

`kubectl get nodes`

Describe specific node:

`kubectl describe nodes node-1`

## Cluster Components

View Kube proxies

`kubectl get daemonSets --namespace=kube-system kube-proxy`

See DNS servers:

`kubectl get deployments --namespace=kube-system core-dns`

Load balancing:

`kubectl get services --namespace=kube-system core-dns`

See the UI server:

`kubectl get deployments --namespace=kube-system kubernetes-dashboard`

Load balancing for dashboard:

`kubectl get services --namespace=kube-system kubernetes-dashboard`

View local UI by running:

`kubectl proxy`

then go to `localhost:8081`