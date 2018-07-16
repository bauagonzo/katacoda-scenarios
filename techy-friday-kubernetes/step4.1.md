Deploy a couchbase cluster and application using couchbase-operator

## Task
Get the couchbase-operator utils
`curl -LO https://github.com/bauagonzo/katacoda-scenarios/raw/master/techy-friday-kubernetes/assets/couchbase/bin/cbopctl /usr/bin`{{execute HOST1}}

`curl -LO https://github.com/bauagonzo/katacoda-scenarios/raw/master/techy-friday-kubernetes/assets/couchbase/bin/cbopinfo /usr/bin`{{execute HOST1}}

`cd couchbase`{{execute HOST1}}

### Security fluff

`kubectl create serviceaccount couchbase-operator --namespace default`{{execute HOST1}}

`kubectl create clusterrolebinding couchbase-operator --clusterrole couchbase-operator --serviceaccount default:couchbase-operator`{{execute HOST1}}

`kubectl create -f secret.yaml`{{execute HOST1}}

### Create the operator

`kubectl create -f operator.yaml`{{execute HOST1}}

`kubectl get deployments -l app=couchbase-operator`{{execute HOST1}}

`kubectl get pods -l app=couchbase-operator`{{execute HOST1}}


### Create the couchbase-nodes
`cbopctl create -f couchbase-cluster.yaml`{{execute HOST1}}

`kubectl get pods`{{execute HOST1}}

Import dataset
`kubectl exec pod cb-example-0000 -ti /opt/couchbase/bin/cbimport json -c 127.0.0.1:8091 -u Administrator -p password -b travel-sample -f sample -d /opt/couchbase/samples/travel-sample.zip`{{execute HOST1}}

### Create the web service with travel-app

`kubectl create -f travel-sample.yaml`{{execute HOST1}}


https://[[HOST2_SUBDOMAIN]]-32000-[[KATACODA_HOST]].environments.katacoda.com/
## Docker bonus

Tweak the travel-app sample to use
`cat Dockerfile`{{execute HOST2}}

`docker build -t bauagonzo/cb-travel-sample:latest .`{{execute HOST2}}
