Deploy a couchbase cluster and application using couchbase-operator

## Task
Get the couchbase-operator utils
`curl -LO https://github.com/bauagonzo/katacoda-scenarios/raw/master/techy-friday-kubernetes/assets/couchbase/bin/cbopctl`{{execute HOST1}}

`curl -LO https://github.com/bauagonzo/katacoda-scenarios/raw/master/techy-friday-kubernetes/assets/couchbase/bin/cbopinfo`{{execute HOST1}}

`chmod +x cbop* && mv cbop* /usr/bin`{{execute HOST1}}

### Security fluff

By default kubernetes has RBAC enabled. So in order to a pod to operate another pod we need to grant the proper roles and privileges
`kubectl create -f couchbase/cluster-role.yaml`{{execute HOST1}}

`kubectl create serviceaccount couchbase-operator --namespace default`{{execute HOST1}}

`kubectl create clusterrolebinding couchbase-operator --clusterrole couchbase-operator --serviceaccount default:couchbase-operator`{{execute HOST1}}

`kubectl create -f couchbase/secret.yaml`{{execute HOST1}}

### Create the operator
Create the Couchbase operator deployment & deploy
`kubectl create -f couchbase/operator.yaml`{{execute HOST1}}

`kubectl get deployments -l app=couchbase-operator`{{execute HOST1}}

Check everything is started properly with
`kubectl get pods -l app=couchbase-operator`{{execute HOST1}}


### Create the couchbase-nodes
`vi  couchbase/couchbase-cluster.yaml`{{execute HOST1}}
`cbopctl create -f couchbase/couchbase-cluster.yaml`{{execute HOST1}}

`kubectl get pods`{{execute HOST2}}

`kubectl describe pods cb-example-0000 | more`{{execute HOST1}}

Import dataset (TODO use a job)

`kubectl exec cb-example-0000 -ti bash`{{execute HOST1}}
`cbc-bucket-create toto  -u Administrator -P password`{{execute HOST1}}
`curl -LO https://github.com/bauagonzo/katacoda-scenarios/raw/master/techy-friday-kubernetes/assets/couchbase/travel-sample-flo.zip`{{execute HOST1}}
`/opt/couchbase/bin/cbimport json -c 127.0.0.1:8091 -u Administrator -p password -b travel-sample -f sample -d ~/travel-sample-flo.zip`

### Create the web service with travel-app

Gives the rights to communicate with other containers, deploy the container, expose the port to the world
`vi couchbase/travel-sample.yaml`{{execute HOST2}}

`kubectl create -f couchbase/travel-sample.yaml`{{execute HOST2}}


`kubectl get pods`{{execute HOST2}}


We can now access the app through : https://[[HOST2_SUBDOMAIN]]-32000-[[KATACODA_HOST]].environments.katacoda.com/
## Docker bonus

Tweak the travel-app sample to use
`cat Dockerfile`{{execute HOST2}}

`docker build -t bauagonzo/cb-travel-sample:latest .`{{execute HOST2}}
