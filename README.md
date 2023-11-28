# px-demo-fb-direct-access

This code will help demonstrate Portworx using Direct Access volumes with FlashBlade.

## Prerequisites: 
* Portworx Enterprise installed
* Portworx enabled for access to a FlashBlade (using pure.json as noted here: https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-pvcs/pure-flashblade/)

## Usage:
Use `px-fb-da-sc.yaml` to create a Direct Access storageclass using FlashBlade, then deploy the RWO and RWX examples.

## Example:
```
kubectl create ns fb-da-demo
kubectl apply -f px-fb-da-sc.yaml
kubectl -n fb-da-demo apply -f deployment-rwo.yaml
kubectl -n fb-da-demo apply -f deployment-rwx.yaml
```

This will create two deployments:
* `deployment-rwo.yaml` creates a single pod running `busybox` and mounting a RWO Direct Access PVC from the FlashBlade at `/mnt/busybox-rwo`
* `deployment-rwx.yaml` creates a deployment with 3 pods running `busybox` and mounting across all 3 pods a single shared RWX Direct Access PVC from the FlashBlade at `/mnt/busybox-rwx`

You can see that all 3 pods are writing to the RWX PVC by getting the output of the file. The following shell script will do this:
```RWX_POD=$(kubectl -n fb-da-demo get pod -l app=busybox-rwx -o jsonpath='{.items[0].metadata.name}')
kubectl -n fb-da-demo exec -ti ${RWX_POD} -- cat /mnt/busybox-rwx/hosts.log
```
These two PVCs show up in the flashblade as different filesystems. This example code creates them with 2 unique sizes to they are readily identifiable. 

One can test the capabilities by execing into the pods and editing files in the mounted Direct Access volumes' filesystems.
 
