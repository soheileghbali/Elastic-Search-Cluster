#### Master Node
The master node is used for supervision as it tracks which node is part of
the cluster or which shards to allocate to which nodes. The master node is
important to maintain a healthy cluster of Elasticsearch. We can configure a
master node by changing a node’s node.master option as true in the
Elasticsearch configuration file. If we want to create a dedicated master
node, we must set other types as false in the configuration. Take a look at
the following code:
```sh
node.master: true
node.voting_only: false
node.data: false
node.ingest: false
node.ml: false
xpack.ml.enabled: true
cluster.remote.connect: false
```

Here, you can see the voting_only option; if we set it false, the node will
work as the master eligible node and can be picked as a master node.
However, if we set the voting_only option as true, the node can participate
in master node selection but cannot become a master node by itself. I will
explain how master node selection works later.

#### Data node
Data nodes are responsible for storing data and performing CRUD
operations on it. It also performs data search and aggregations. We can
configure a data node by changing a node’s node.data option to true in the
Elasticsearch configuration file. If we want to create a dedicated data node,
we must set other types as false in the configuration. Refer to the following
code:
```sh
node.master: false
node.voting_only: false
node.data: true
node.ingest: false
node.ml: false
cluster.remote.connect: false
```
Here, we are setting the node.data to true and all other options to false.

#### Ingest Node
Ingest nodes are used to enrich and transform data before indexing it. So,
they create an ingest pipeline using which data is transformed before
indexing. We can configure an ingest node by changing a node’s
node.ingest option to true in the Elasticsearch configuration file. Any
node can work as an ingest node, but if we have heavy data that we want to
ingest, it is recommended to use a dedicated ingest node. To create a
dedicated ingest node, we must set other types as false in the configuration.
Refer to the following code:
```sh
node.master: false
node.voting_only: false
node.data: false
node.ingest: true
node.ml: false
cluster.remote.connect: false
```
Here, we are setting the node.ingest to true and all other options to false.

#### Machine learning node
Elastic machine learning is not freely available, so if xpack.ml.enabled is
set to true, we can create a machine learning node by changing the node.ml
option to true. If we want to run machine learning jobs, we must change at
least one node in the cluster as a machine learning node. To create a
dedicated machine learning node, we must set other types as false in the
configuration. Here’s the code:
```sh
node.master: false
node.voting_only: false
node.data: false
node.ingest: false
node.ml: true
xpack.ml.enabled: true
cluster.remote.connect: false
```
So, we can change the node type to any of the preceding options, but a node
has all the types by default.




