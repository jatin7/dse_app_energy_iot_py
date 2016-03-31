###Background
This repo is the technical how-to guide for running a sample Energy IoT application leveraging DataStax Enterprise. For background on the use case and architecture please see the following - (https://gist.github.com/pbayliss/9a27b069e7b28bbf67a9f00806ebf170)

### Pre-requisites:
```pip install cassandra-driver```  
Confluent Kafka platform: (http://www.confluent.io/developer)

### Setup:
Start DSE, with both Search and Analytics enabled (analytics only used for now, search coming later)  
```bin/dse cassandra -k -s```

From Confluent dir:
Start zk, kafka, schema registry, and rest server:  
```
bin/zookeeper-server-start etc/kafka/zookeeper.properties
bin/kafka-server-start etc/kafka/server.properties
bin/schema-registry-start ./etc/schema-registry/schema-registry.properties
bin/kafka-rest-start
```

##### Create topic:
`./bin/kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic meter_readings`

### Usage:
To insert data into Cassandra, run:  
`python energy.py <number of devices> <cassandra host ip>`

For example:  
`python energy.py 3 127.0.0.1`  

To generate a file containing a day's data for sensors:  
`python filegen.py <number of devices>`

For example:  
`python filegen.py 1000`  

To simulate pushes of data using REST URL's run:  
`python generaterest.py <number of devices>`

For example:  
`python generaterest.py 1000`  

To run the batch job:  
`<dse install directory>/bin/dse spark-submit <path to batch.py>`  

For example:  
`/dse/bin/dse spark-submit ./batch.py`

Testing:  
`bin/kafka-console-consumer --zookeeper localhost:2181 --topic meter_readings --from-beginning\`
