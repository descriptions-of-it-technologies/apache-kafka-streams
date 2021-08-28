# Apache Kafka Streams.



## What is Kafka Streams
* Easy date processing and transformation library within Kafka
* Standard Java Application.
* No need to create a separate cluster.
* Highly scalable, elastic and fault tolerant
* Exactly once Capabilities.
* One record at a time processing (no batching)
* Works for any application size.



## Kafka Streams vs Spark Streams, NiFi, Flink
* Micro batch vs per data streaming.
* Cluster required vs no cluster required.
* Scales easy by just adding java processes (no re-configuration requiere)
* Exactly once semantic (vs at least once for Spark)
* All code based.



## Kafka Streams application terminology
* A `stream` is a sequence of immutable data records, that fully ordered, can be replayed, and is fault 
  tolerance (think of a Kafka Topic as a parallel)
* a `stream processor` is a node in the processor topology (graph). It transforms incoming streams, record by record, and 
  may create a new stream from it.
  * a `source processor` is a special processor that take its data directly from `Kafka Topic`. It has no predecessors 
    in a topology, and dosen't transform the data.
  * a `sink processor` is processor that does not have children, it sent the stream data directly to a `Kafka Topic`
* A `topology` is a graph of processors chained together by streams.



## Kafka Streams application writing a terminology
* `High level DSL`
  * it is simple
  * it has all the operations we need to transform most transformation tasks
  * it contains a lot of syntax helpers to make our life easy
  * it's descriptive
* `Low Level Processor API` 
  * it's an imperative API
  * can be used to implement the most complex logic, but it's rarely needed.



## Streams app properties.
* a stream application, when communicating to Kafka , is leveraging the Consumer and Producer API.
* therefore, all the configurations we learned before are still applicable
* `bootstrap.servers` need to connect to Kafka (usually port 9092)
* `auto.offset.reset.config` set to `erliest`to consume the topic from start
* `application.id` specific to streams application, will be use for:
  * consumer `group.id` == `application.id` (most important one to remember)
  * default `client.id` prefix
  * prefix to internal changelog topics
  * `default.key.serde` for serialisation and deserialization of key
  * `default.value.serde` for serialisation and deserialization of value



## 
* Remember data in `Kafka Streams` is `key` and `value`
* `Stream`
* `MapValues` 
* `FlatMap Values` 
* `SelectKey`
* `GroupByKey`
* `Count`
* `To`



## Printing the Topology
* printing the topology at the start of the application is helpful while developing(and even in production) at it helpful
  understand the application flow directly from the first lines of the logs.
* Reminder: The topology represents all the streams and processors of your Streams application



## Internal topics
* running a Kafka Streams may eventually create internal intermediary topics.
* two types:
  * repartitioning topics: in case you start transforming the key of your stream, a repartitioning will happen at some processor.
  * changelog topics: in case you perform aggregations, Kafka Streams will save compacted data in these topics.
* internal topics:
  * are managed by Kafka Streams
  * are used by Kafka Streams to save/restore state and re-partition data
  * are prefixed by `application.id` parameter
  * should never be deleted, altered or published to.



## Scaling our application
* Our input topic has 2 partitions, therefore we can launch up to 2 instances of our application in parallel without 
  any changes in the code
* this is because a Kafka Streams application relies on Kafka Consumer, and we say in the Kafka Basics course that we 
   could add consumers to a consumer group by just running the same code.
* this makes scaling super easy, without the need of any application cluster
