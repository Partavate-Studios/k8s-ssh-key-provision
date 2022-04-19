# dssh - Keep your authorized\_keys up to date on a Kubernetes cluster. 

## Purpose

Linode's Kubernetes Engine lacks the ability to provision account SSH keys 
onto cluster Linode hosts. This automatically provisions keys on the underlying 
hosts in Kubernetes clusters.

## Ephemerality Warning

Managed Kubernetes providers perform upgrades by deleting and
recreating your VMs. You must not assume that anything you do
on these VMs will persist. In fact, the VM could be deleted
while you are working on it.

Kubernetes hosts are "cattle" not "pets", so anything manually created there will be lost.

## Namespaces

The upstream project ([asauber/dssh](https://github.com/asauber/dssh)) puts this is the `kube-system` 
namespace. This fork instead uses `infrastructure`, as `kube-system` should not be messed with, 
much like John McAfee's dogs.

## Usage

### Deploying Your Public Keys

```
cp keys.yaml.example keys.yaml

# Edit keys.yaml to include your public keys

kubectl apply -f daemonset.yaml
kubectl apply -f keys.yaml
```

### Updating Your Public Keys

```
# Edit keys.yaml to update your public keys

kubectl apply -f keys.yaml
```

### Updating dssh

```
kubectl apply -f daemonset.yaml
```

### Removing dssh
```
kubectl delete daemonset -n infrastructure root-ssh-manager
```

### Notes

- Every 60 seconds your public keys will be applied to all Nodes in the cluster.
- You need permission to deploy priviledged pods.

