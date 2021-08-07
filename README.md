# Cluster API Lab Notes



## Terms:

`Managment Cluster`: Kubernetes cluster where Cluster API has been installed.

`Workload Cluster`: Kubernetes cluster that has been created and is being manged by Cluster API via the Managment Cluster.

`Bootstrap Cluster`: Temporary cluster used when creating a Managemnet cluster.

`Provider`: Cluster APi uses the idea of a provider to interact with underlying IaaS platforms, such as AWS, vSphere, or Azure. providers
have abreviations like CAPA (Cluster API Provider for AWS), CAPZ (Cluster API Provider for Azure), and CAPV (Cluster API Provider for vSphere).

`clusterctl`: A command line tool for performing certain Cluster API related operations.

## Clusterctl:


To initialize a mangement cluster.

```
clusterctl init
```

To upgrade a management cluster's API components.
```
clusterctl upgrade
```

To move Cluster API objects between management clusters.
```
clusterctl move
```

To create workload clusters.
```
clusterctl config
```

## Install  Clusterctl:
 
Install clusterctl binary with curl on linux

Download the latest release; on linux, type:

```
curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/v0.4.0/clusterctl-linux-amd64 -o clusterctl
Make the clusterctl binary executable.


chmod +x ./clusterctl
Move the binary in to your PATH.


sudo mv ./clusterctl /usr/local/bin/clusterctl
Test to ensure the version you installed is up-to-date:


clusterctl version
```

## Establish a Management Cluster:

Use `clusterctl init` on an existing Kubernetes cluster.


Use `clusterctl init` on a temporary bootstrap cluster (often created using KinD), use Cluster API
to create a new cluster, then `clusterctl move` (also known as bootstrap and pivot).

### (Pre flight) commands:

```
export KUBECONFIG=<...>
```

```
Kubectl get nodes

Kubectl get crds

kubectl get crds | grep 'cluster.x-k8s.io'

kubectl api-resources --api-group='cluster.x-k8s.io'

kubectl get ns

kubectl get clusters
```

 These commands should have show us Cluster API has not been installed.
 

### (Create cluster) commands:

 ```
 clusterctl init --infrastucture aws
 ```
 
 Success!!
 
 
### (Post flight) commands:

These commands should now show us Cluster API has been installed.

```
kubectl get ns

kubectl get crds | grep 'cluster.x-k8s.io'

kubectl api-resources --api-group='cluster.x-k8s.io'
```


This should be blank, but it's not giving us an error anymore.

```
kubectl get clusters
```




## What you can do:

Create new Kubernetes clusters in a declarative fashion.

Have Cluster API manage the assosiated infrastructure.

Scale clusters up or down.

Upgrade clusters to a new Kubernetes release.

Destroy clusters and all associated infrastucture, if desired.


## Commands:

View Cluster API CRDs and Kinds

```
kubectl get crds | grep 'cluster.x-k8s.io'

kubectl api-resources --api-group='cluster.x-k8s.io'
```

Handy commands

```
kubectl get machinedeployment

kubectl scale --replicas=3 machinedeployment <machinedeployment name>

Kubectl get machines
```


General CRD commands

```
kubectl api-resources 

kubectl api-resources --api-group=''

kubectl api-resources --api-group='apps'

kubectl api-resources --api-group='networking.k8s.io'

kubectl get crds

kubectl describe crds
```



## Providers:

A Provider is just an additional set of CDRs and controllers. 

This aproach allows the core Cluster API project to remain platform agnostic, while still taking advantage of the functionality enabled 
by each infrastructure provider.


### (AWS example - CAPA): 

commands:

```
kubectl get crds | grep 'infrastructure'

kubectl get ns

(capa-system)

kubectl -n capa-system get pods (to view the capa controller)
```

### (Cluster API Bootstrap Provider for KubeADM example - CABPK):

This Provider drives KubeADm to bootstrap Kubernetes clusters.

It uses KubeADm related objects like KubeadmControlPlane, KubeadmConfig, or a KubeadmConfig Template

<ClusterCTL Yaml File Place Holder>
