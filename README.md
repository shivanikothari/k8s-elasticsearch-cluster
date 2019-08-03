# k8s-elasticsearch-cluster
Deploy an elasticsearch cluster on kubernetes platform.

# Abstract
What is an Elasticsearch cluster?
As the name implies, an Elasticsearch cluster is a group of one or more Elasticsearch nodes instances that are connected together. The power of an Elasticsearch cluster lies in the distribution of tasks, searching and indexing, across all the nodes in the cluster.

The nodes in the Elasticsearch cluster can be assigned different jobs or responsibilities:

Data nodes — stores data and executes data-related operations such as search and aggregation
Master nodes — in charge of cluster-wide management and configuration actions such as adding and removing nodes
Client nodes — forwards cluster requests to the master node and data-related requests to data nodes
Ingest nodes — for pre-processing documents before indexing

# Getting Started

## Step 1: Check k8s cluster
Make sure that kubernetes cluster is up and running.
````
$ kubectl get nodes
NAME                 STATUS     ROLES    AGE    VERSION
k8s-master           Ready      master   2d    v1.14.2
elk-node-1           Ready      slave    2d    v1.14.2
elk-node-2           Ready      slave    2d    v1.14.2
elk-node-3           Ready      slave    2d    v1.14.2
````

## Step 2: Creating namespace for elasticsearch cluster
Create a separte namespace for es cluster in order to distinguish it from rest of the deployments.
````
$ kubectl create -f namespace.yml
````

## Step 3: Create persistent volumes
The next step is create persistent volumes used by the data nodes. Pick whichever suits your platform. Alternatively, you can look for a more suitable StorageClass [here]: https://kubernetes.io/docs/concepts/storage/storage-classes/. 
````
$ kubectl create -f pv/
````

## Step 4: Deploy elasticsearch cluster
This will create a StatefulSet for elasticsearch-data and master and a deployment for elasticsearch-ingest.
````
$ kubectl create -f es-master.yml
$ kubectl create -f es-data.yml
$ kubectl create -f es-ingest.yml
$ kubectl create -f es-services.ymlsav
````

## Step 4: Verification
````
$ kubectl get pods -n es
NAME                                   READY   STATUS    RESTARTS   AGE
elasticsearch-data-0                   1/1     Running   0          1d
elasticsearch-data-1                   1/1     Running   0          1d
elasticsearch-ingest-5fdc879fb-2tn6w   1/1     Running   0          1d
elasticsearch-ingest-5fdc879fb-tvzfg   1/1     Running   0          1d
elasticsearch-master-0                 1/1     Running   0          1d
elasticsearch-master-1                 1/1     Running   0          1d
elasticsearch-master-2                 1/1     Running   0          1d
````

## Elasticsearch Cluster APIs
Elasticsearch supports a large number of cluster-specific API operations that allow you to manage and monitor your Elasticsearch cluster. Most of the APIs allow you to define which Elasticsearch node to call using either the internal node ID, its name or its address. Below is a list of a few of the more basic API operations you can use. 

### Cluster Health:
This API can be used to see general info on the cluster and gauge its health.
````
$ curl -XGET 'localhost:9200/_cluster/health?pretty'
````

### Cluster State
This API can be used to see a detailed status report of the entire cluster.
````
$ curl -XGET 'localhost:9200/_cluster/state?pretty'
````

### Cluster Stats
This API helps in monitoring performance metrics of the entire cluster.
````
$ curl -XGET 'localhost:9200/_cluster/stats?human&pretty'
````

### Nodes Stats
In order to inspect the elastic nodes we can use below API.
````
$ curl -XGET 'localhost:9200/_nodes/stats?pretty'
````
