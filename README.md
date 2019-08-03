# k8s-elasticsearch-cluster
Deploy an elasticsearch cluster on kubernetes platform.

# Getting Started

Step 1: Check k8s cluster

$ kubectl get nodes
NAME                 STATUS     ROLES    AGE    VERSION
k8s-master           Ready      master   2d    v1.14.2
elk-node-1           Ready      slave    2d    v1.14.2
elk-node-2           Ready      slave    2d    v1.14.2
elk-node-3           Ready      slave    2d    v1.14.2

Step 2: Creating namespace for elasticsearch cluster
Create a separte namespace for es cluster in order to distinguish it from rest of the deployments.

$ kubectl create -f namespace.yml

Step 3: Create persistent volumes
$ kubectl create -f volume/

Step 4: Deploy elasticsearch cluster
This will create a deployment for elasticsearch-master and a StatefulSet for elasticsearch-data and deployment for elasticsearch-ingest

$ kubectl create -f es-master.yml
$ kubectl create -f es-data.yml
$ kubectl create -f es-ingest.yml

Step 4: Verification

$ kubectl get pods





