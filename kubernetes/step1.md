Etcd is a "Distributed reliable key-value store for the most critical data of a distributed system". Kubernetes uses Etcd to store state about the cluster and service discovery between nodes. This state includes what nodes exist in the cluster, which nodes they are running on and what containers should be running.

The command below will launch a single node etcd cluster listening on port 4001.
`
kubectl run demo --image=couchbase:latest -p 8091-8094:8091-8094 -p 11210-11211:11210-11211
`{{execute}}

The _net=host_ means the container will share the same network as the host, removing the need to map ports.

In production you would want to run etcd on three separate machines to ensure maximum availability.
