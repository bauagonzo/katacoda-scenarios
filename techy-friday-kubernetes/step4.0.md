We have now a cluster of 2 nodes with network communication in place.
In that step, we will create the persistent storage.

## Task

The storage management within the cluster operates two resources, a PersistentVolume to be added by the admin and the PersistentVolumeClaim to be consumed by the users.

PV and PVC can be provisioned statically (like below) or dynamically.

`cat local-pv.yaml`{{execute HOST1}}


`kubectl create -f local-pv.yaml`{{execute HOST1}}

`kubectl get persistentvolume`{{execute HOST2}}

Note that the persistent local volume is only accessible by the master node. For real shared storage we should use another storage provider with such capabilities.

## Bonus

FC configuration

`cat local-fc.yaml`{{execute HOST1}}
