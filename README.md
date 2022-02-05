# fb-direct-access-demo

This code will help demonstrate Portworx using Direct Access volumes with FlashBlade.

Prerequisites: Portworx Enterprise installed
Portworx enabled for access to a FlashBlade (using pure.json as noted here: https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-pvcs/pure-flashblade/)

Use `px-fb-da-sc.yaml` to create a Direct Access storageclass using FlashBlade, then deploy the RWO and RWX examples.

Example:
```
kubectl apply -f px-fb-da-sc.yaml
kubectl apply -f deployment-rwo.yaml
kubectl apply -f deployment-rwx.yaml
```

This will create two deployments:
 `deployment-rwo.yaml` creates a single pod running `busybox` and mounting a RWO Direct Access PVC from the FlashBlade at `/mnt/busybox-rwo`
 `deployment-rwx.yaml` creates a deployment with 3 pods running `busybox` and mounting a RWX Direct Access PVC from the FlashBlade at `/mnt/busybox-rwx`

These two PVCs show up in the flashblade as different filesystems. This example code creates them with 2 unique sizes to they are readily identifiable. 

One can test the capabilities by execing into the pods and editing files in the mounted Direct Access volumes' filesystems.
 
