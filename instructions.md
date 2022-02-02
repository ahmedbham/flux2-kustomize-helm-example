# Instructions

## Setup

* Run:

```cli

az feature register --namespace Microsoft.ContainerService --name AKS-ExtensionManager

az version
az upgrade

az provider register --namespace Microsoft.Kubernetes
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.KubernetesConfiguration

az extension add -n k8s-configuration
az extension add -n k8s-extension

az configure --defaults group=vn-take4 location=eastus
aks_cluster_name=vn-take4
az aks get-credentials -n $aks_cluster_name

az k8s-configuration flux create -c $aks_cluster_name -n gitops-demo --namespace gitops-demo -t managedClusters --scope cluster -u https://github.com/ahmedbham/flux2-kustomize-helm-example --branch main  --kustomization name=infra path=./infrastructure prune=true --kustomization name=apps path=./apps/staging prune=true dependsOn=["infra"]

az k8s-configuration flux show  -c $aks_cluster_name -n gitops-demo -t managedClusters

kubectl get namespaces

kubectl -n nginx port-forward svc/nginx-ingress-controller 8080:80

curl -H "Host: podinfo.staging" http://localhost:8080 {"hostname": "podinfo-59489db7b5-lmwpn", "version": "5.0.3"}


```

* 