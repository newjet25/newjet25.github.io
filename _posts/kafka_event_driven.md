[Understanding Kafka and event driven processing:]{.underline}

Understanding Kafka: Kafka (Apache Kafka) is an event streaming platform
purposed to receive, store and publish event messages based on the
Publisher- Subscriber model.

Some of the core concepts involved in this are:

Level 1: Basic

Consumer

Topic

Producer

So, this is the simplistic level with 3 actors:

Producer: Responsible for publishing messages to the topic based on the
event it is handling.

Topic: Topic represents logical categories of events and it is an append
only log.

Consumer: Responsible for consuming message from the topic. This remains
independent of the producer.

**Key point:** Producers and consumers are decoupled. Producers don't
need to know who consumes, and consumers don't care how data was
produced.

Level 2: Parallelism & Fault Tolerance

Producer

Consumer Groups

Partition 0

Partition 1

Partition 2

Topic

Here, we can scale now in parallel with partition in topics:

Partitions: A topic can be divided into partitions for parallel
processing. Partition can be done based on key or without key. Events in
a partition are always ordered.

Replication: Each partition has one leader and multiple replicas. When
leader fails, replicas in the in-sync replica set takes position. This
managed by Kafka controller.

Consumer Groups: Group of consumers consume from a topic
collaboratively. 1 partition can be only consumed by 1 consumer in the
group.

Offsets: Consumer track progress using offsets. These can be
communicated manually or automatically.

Level 3: Delivery Semantics

Kafka guarantees event delivery, but app logic or configuration defines
how "reliable" that delivery could be.

At-most once: In this case, offset is committed before processing 🡪 no
duplicates. This can be achieved by ack=0 or ack=1 in producer. This
method is fastest, but possible message loss.

At-least once: In this case, offset is committed after processing 🡪
duplicate possible. This can be achieved by acks=all in producer.

Exactly-once: In this case, exactly once message delivery is done. This
can be achieved by acks=all and enable.idempotence=true

Key: Delivery Semantics are not topic level, but come from
producer-consumer configuration and application logic

Level 4: Design Patterns and trade-offs

Key components when creating Design patterns are:

- Partition the topic based on keys as these enables ordering and avoid
  random message distribution.

- Create Dead letter queues to store failed transaction and use for
  retry/fail analysis.

- Producers should retry transient failures and Consumer should have
  retry topics to use to avoid processing without loss.

Key: Design patterns should focus on tradeoffs while balancing Ordering,
reliability and flexibility.

Level 5: Performance Tuning

Some Key practices to ensure better performance:

- Used Acks mode as needed and described in Level 4

- Reduce or Eliminate Customer lag by building efficient consumers. Lag
  = Latest offset written by producer -- Committed Offset by consumer.

- Reduce backpressure on Kafka topic by controlling commit frequency and
  task throughput.

------------------------------------------------------------------------

Level 6: Operations and Governance

Some key practices to make the Kafka usage enterprise-grade and
resilient:

- Use of Schema Registry for the schema compliant topics and message
  handling.

- Use of different techniques as mentioned above for scaling
  infrastructure help to make systems resilient.

- Monitoring Customer lags, infrastructure saturation and automatic
  notification would help for resilient systems.
