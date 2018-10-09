Deploy a couchbase cluster and application using couchbase-operator

## Task
Get the couchbase-operator utils
`curl -LO https://github.com/bauagonzo/katacoda-scenarios/raw/master/techy-friday-kubernetes/assets/couchbase/bin/cbopctl`{{execute HOST1}}

`curl -LO https://github.com/bauagonzo/katacoda-scenarios/raw/master/techy-friday-kubernetes/assets/couchbase/bin/cbopinfo`{{execute HOST1}}

`chmod +x cbop* && mv cbop* /usr/bin`{{execute HOST1}}

`cd couchbase`{{execute HOST1}}

### Security fluff

By default kubernetes has RBAC enabled. So in order to a pod to operate another pod we need to grant the proper roles and privileges
`kubectl create -f cluster-role.yaml`{{execute HOST1}}

`kubectl create serviceaccount couchbase-operator --namespace default`{{execute HOST1}}

`kubectl create clusterrolebinding couchbase-operator --clusterrole couchbase-operator --serviceaccount default:couchbase-operator`{{execute HOST1}}

`kubectl create -f secret.yaml`{{execute HOST1}}

### Create the operator
Create the Couchbase operator deployment & deploy
`kubectl create -f operator.yaml`{{execute HOST1}}

`kubectl get deployments -l app=couchbase-operator`{{execute HOST1}}

Check everything is started properly with
`kubectl get pods -l app=couchbase-operator`{{execute HOST1}}


### Create the couchbase-nodes
`vi  couchbase-cluster.yaml`{{execute HOST1}}
`cbopctl create -f couchbase-cluster.yaml`{{execute HOST1}}

`kubectl get pods`{{execute HOST2}}

Import dataset (TODO use a job)
`kubectl exec cb-example-0000 -ti bash`{{execute HOST1}}

`/opt/couchbase/bin/cbimport json -c 127.0.0.1:8091 -u Administrator -p password -b travel-sample -f sample -d /opt/couchbase/samples/travel-sample.zip`{{execute HOST1}}

### Create the web service with travel-app

`kubectl create -f travel-sample.yaml`{{execute HOST1}}

Expose the port to the world
`kubectl create -f nodeport.yaml`{{execute HOST2}}

We can now access the app through : https://[[HOST1_SUBDOMAIN]]-32000-[[KATACODA_HOST]].environments.katacoda.com/
## Docker bonus

Tweak the travel-app sample to use
`cat Dockerfile`{{execute HOST2}}

`docker build -t bauagonzo/cb-travel-sample:latest .`{{execute HOST2}}
