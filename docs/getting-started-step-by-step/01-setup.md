# Setup: Setting up Crossplane on Kubernetes.

The goal in this part is to install the Crossplane Runtime and necessary tooling to get started:

* A) Create a local Kubernetes cluster 
* B) Install the Crossplane CLI
* C) Install Crossplane Runtime

<details><summary>Prerequisites</summary>

You should bring basic Kubernetes and cloud provider knwowlege as we will use Crossplane to connect both. Also please make sure you have [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) and [Helm](https://helm.sh/docs/intro/install/) installed.
</details>

## A) Create a local Kubernetes cluster 

Our first step is to create a local Kubernetes cluster with kind:

### Run:
```bash
# Install a local https://kind.sigs.k8s.io cluster:
$ kind create cluster --image kindest/node:v1.16.15 --wait 5m
```

### Check:
```
# List all local available cluster. Will be just one named "kind" at this point.
$ kind get clusters

# To delete the cluster later:
# $ kind delete cluster
```

## B) Install the Crossplane CLI

The Crossplane CLI extends kubectl to install Crossplane packages like Providers or Configurations:

### Run:
```bash
# Install the Crossplane CLI via shell script:
$ curl -sL https://raw.githubusercontent.com/crossplane/crossplane/master/install.sh | sh
```

### Check:
```bash
# Check that the Crossplane CLI got installed:
$ kubectl crossplane -h
```

## C) Install Crossplane Runtime

In our last setup step we will install the Crossplane runtime into the cluster:

### Run:
```bash
$ kubectl create namespace crossplane-system
$ helm install crossplane --namespace crossplane-system crossplane-stable/crossplane --version 1.2.1
```

### Check:
```bash
# Show the installed Helm chart:
# (check for deployed status)
$ helm list -n crossplane-system

# Show the crossplane system resources:
# - apps/crossplane
# - apps/crossplane-rbac-manager 
# (check for running / ready)
$ kubectl get all -n crossplane-system
```

## Next

After we have setup a Crossplane runtime we want to [Using Managed Resources from AWS](02a-managed-resources-aws.md): Connect Crossplane to manage AWS resources. 

## More Info

<details><summary>Creating a Clusters</summary>

We just used kind as it is currently our favourite way to install Kubernetes cluster. Other install options like minikube or kubeadm or using an existing cluster that someone can provide. If you don't want to manage controlplane yourself you can also check out hosted solutions like [Upbound Cloud](https://www.upbound.io/cloud). 
</details>

<details><summary>On the Crossplane CLI</summary>

See the [Install Reference](https://crossplane.io/docs/v1.2/reference/install.html) for more information.
</details>

<details><summary>On the Crossplane Runtime</summary>

- TODO link to the several CRDs
- CRDs in general [https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
</details>