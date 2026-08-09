# SimpleKafka (Educational Kafka-like implementation)

A small, educational Kafka-like broker and client library implemented in Java.

**Overview**
- Minimal broker implementation with topic/partition management and simple socket protocol.
- Client library to create topics, produce (send) and fetch messages.
- Includes a lightweight ZooKeeper client for metadata (expects a running ZooKeeper instance).

**Prerequisites**
- Java 11+ (JDK)
- Maven
- ZooKeeper running (default port `2181`) if you plan to use cluster features

**Build**

```bash
mvn package
```

**Run a broker (example)**

After building, start a broker (brokerId host port [zkPort]):

```bash
java -cp target/classes com.simplekafka.broker.SimpleKafkaBroker 1 127.0.0.1 9092 2181
```

Alternatively (if you prefer Maven exec):

```bash
mvn -q exec:java -Dexec.mainClass="com.simplekafka.broker.SimpleKafkaBroker" -Dexec.args="1 127.0.0.1 9092 2181"
```

**Using the client library (example)**

There is no CLI producer included — use the `SimpleKafkaClient` API from your Java code. Example:

```java
SimpleKafkaClient client = new SimpleKafkaClient("127.0.0.1", 9092);
client.initialize();
// Create topic with 1 partition and replication factor 1
client.createTopic("my-topic", 1, (short)1);
long offset = client.send("my-topic", 0, "hello world".getBytes());
List<byte[]> messages = client.fetch("my-topic", 0, 0, 4096);
```

**Notes & limitations**
- This is an educational, simplified implementation — not production-ready.
- The `SimpleKafkaConsumer` file is currently empty; use `SimpleKafkaClient` directly to implement consumers/producers.
- ZooKeeper is used for metadata; ensure ZooKeeper is running on the configured port.

**Project structure**
- [src/main/java/com/simplekafka/broker](src/main/java/com/simplekafka/broker) — broker classes (controller logic, partitions, protocol)
- [src/main/java/com/simplekafka/client](src/main/java/com/simplekafka/client) — client API

**Next steps / ideas**
- Implement a CLI producer/consumer example app.
- Add integration tests that start an embedded ZooKeeper and broker.

---

Created from the current workspace sources. For implementation details see the broker and client classes in `src/main/java/com/simplekafka/`.
