# Energy IoT
## Use Case
An energy supplier has a large customer base and a network of Smart Meters for each of them. There is a significant amount of data being generated and collected by these Smart Meters, which they would like to be able to ingest, store, and retrieve for their own uses. Currently they wish to start testing with the assumption that there are currently 10 million devices, but they are expecting a total number of 50 million devices down the road. Each Smart Meter collects a reading every 15 minutes (a total of 96 values per day). Initially 90% of the data will be batch loaded nightly, while the other 10% will stream the data as it is collected in real-time. 

The Energy Company woudl like the following:
1.	A REST API to show how real-time values will be delivered and stored.
2.	An efficient way to store data going forward.
3.	An indication on how long it will take to load 10 million new rows per day for daily loads.
4.	A REST interface to get all values for a device between 2 dates.
5.	A REST interface to get an aggregated view over 1 week, 1 month, 3 months, 6 months or a year.

## Setup
DataStax Enterprise (DSE) will be the platform for this Energy IoT cloud application. This qualifies as a cloud application since it will be distributed in terms of data collection and interaction, it must always be available, a core requirement is to scale easily and it must perform at low latencies. The DSE platform supports IoT use cases very well.  The high throughput, low latency needs are core to DSE. Along with that are capabilities of DSE Analytics to aid in streaming data and aggregations, as well as DSE Search for flexible querying options (like time-based or geospatial-based queries).



Pre-requisites:
pip install cassandra-driver
confluent kafka platform: http://www.confluent.io/developer

SETUP:
#start dse, with both search and analytics enabled (analytics only used for now, search coming later)
bin/dse cassandra -k -s

From confluent dir:
#start zk, kafka, schema registry, and rest server
bin/zookeeper-server-start etc/kafka/zookeeper.properties
bin/kafka-server-start etc/kafka/server.properties
bin/schema-registry-start ./etc/schema-registry/schema-registry.properties
bin/kafka-rest-start

#create topic
./bin/kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic meter_readings

USAGE:

To insert data into Cassandra, run:
python energy.py <number of devices> <cassandra host ip>

For example:
python energy.py 3 127.0.0.1

To generate a file containing a day's data for sensors:

python filegen.py <number of devices>

For example:

python filegen.py 1000

To simulate pushes of data using REST urls,s run

python generaterest.py <number of devices>

For example

python generaterest.py 1000

To run the batch job:

<dse install directory>/bin/dse spark-submit <path to batch.py>

For example:

/dse/bin/dse spark-submit ./batch.py


testing:

bin/kafka-console-consumer --zookeeper localhost:2181 --topic meter_readings --from-beginning\
