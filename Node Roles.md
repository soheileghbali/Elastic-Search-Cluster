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
in master node selection but cannot become a master node by itself.

The master node's tasks are primarily used for lightweight
cluster-wide operations, including creating or deleting an index, tracking the
cluster nodes, and determining the location of the allocated shards. By default,
the master-eligible role is enabled. A master-eligible node can be elected to
become the master node (the node with the asterisk) by the master-election
process. You can disable this type of role for a node by setting node.master to
false in the elasticsearch.yml file

🟥 The number of master nodes must always be odd to prevent split brain problem.

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

A data node contains data that contains indexed documents. It
handles related operations such as CRUD, search, and aggregation. By default,
the data node role is enabled, and you can disable such a role for a node by
setting the node.data to false in the elasticsearch.yml file.

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

Using an ingest nodes is a way to process a document in pipeline
mode before indexing the document. By default, the ingest node role is
enabled—you can disable such a role for a node by setting node.ingest to
false in the elasticsearch.yml file.

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

#### Coordinating-only node (client node)
If all three roles (master eligible, data, and ingest) are
disabled, the node will only act as a coordination node that performs routing
requests, handling the search reduction phase, and distributing works via bulk
indexing.
This node acts as a search load balancer (fetching data
from nodes, aggregating results, and so on). This kind of
node is also called a coordinator or client node.

To prevent the queries and aggregations from creating instability in your
cluster, coordinator (or client/proxy) nodes can be used to provide safe
communication with the cluster.
How to set a node as cordinator:
```sh
node.master: false
node.data: false
```

### There's more…
Related to the number of master nodes, there are settings that require at least half of
them plus one to be available to ensure that the cluster is in a safe state (no risk of
split brain: https:/​/​www.​elastic.​co/​guide/​en/​elasticsearch/​reference/​6.​4/
modules-​node.​html#split-​brain). This setting is discovery.zen.minimum_master_nodes, and it must be set to the following equation:
```sh
(master_eligible_nodes / 2) + 1
```
🟥 To have a High Availability (HA) cluster, you need at least three nodes that are masters with the value of minimum_master_nodes set to 2.

🟥 The best practice is to have a pool of coordinator nodes with ingestion enabled to
provide the best safety for the cluster and ingestion pipeline.

![Elastic Search Cluster](https://github.com/soheileghbali/Elastic-Search-Cluster/blob/main/Elastcic%20Search%20Architecture.jpg)


