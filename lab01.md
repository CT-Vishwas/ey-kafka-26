# Single Cluster kafka Setup

This guide will walk you through setting up a single Kafka cluster.
## Prerequisites
- Java 17 or higher installed
```bash
sudo apt install openjdk-17-jdk
java -version
```
- Apache Kafka downloaded and extracted
  
## Steps
1. Download Kafka & extract it.

```bash
wget https://www.apache.org/dyn/closer.lua/kafka/4.3.1/kafka_2.13-4.3.1.tgz?action=download

mv https://www.apache.org/dyn/closer.lua/kafka/4.3.1/kafka_2.13-4.3.1.tgz?action=download kafka_2.13-4.3.1.tgz

mkdir kafka
cd kafka

tar -xvf ../kafka_2.13-4.3.1.tgz --strip 1
ls
```

2. Generate a Cluster UUID

```bash
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```
This initializes Kafka's storage metadata. Do not reformat an existing data directory containing messages you want to keep.

3. Format Log Directories

```bash
bin/kafka-storage.sh format --standalone -t $KAFKA_CLUSTER_ID -c config/server.properties
```

4. Start the Kafka Server

```bash
bin/kafka-server-start.sh config/server.properties
```
Keep this terminal session running. Open additional SSH sessions to execute the CLI commands below.
5. Create a topic to store your events
In this step, you will create a topic named `quickstart-events` to store your events.
```bash
bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

6. Describe the topic

```bash
bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```

7. Write some events into the topic
Before reading the events, make sure you have written some events into the topic using the producer.
```bash
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
>This is my first event
>This is my second event
```

8. Read the events
In a new Terminal window, run the following command to read the events from the topic.
```bash
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```

You should see the events you wrote earlier displayed in the terminal.

## Cleanup
After you are done with the Kafka cluster, you can stop the Kafka server to free up resources.
```bash
bin/kafka-server-stop.sh
```

Remove the Kafka directory if you no longer need it.
```bash
cd ..
rm -rf kafka
```

# Exploring Kafka Topics

Once you have created and interacted with your Kafka topics, you can explore them further using various Kafka CLI commands.

## Check Connectivity

```bash
bin/kafka-broker-api-versions.sh --bootstrap-server localhost:9092
```

## Export bootstrap server

```bash
export BS=localhost:9092
```

## Create and inspect a topic

```bash
bin/kafka-topics.sh \
  --create \
  --topic cyber-events \
  --partitions 3 \
  --replication-factor 1 \
  --bootstrap-server "$BS"
```
Why these settings?
- `--topic`: topic name.
- `--partitions 3`: allows messages to be distributed across three partitions.
- `--replication-factor 1`: stores each partition on the single broker.

## Describe a topic
```bash
bin/kafka-topics.sh --describe --topic cyber-events --bootstrap-server "$BS"
```

## List all topics
```bash
bin/kafka-topics.sh --list --bootstrap-server "$BS"
```

## Produce messages

```bash
bin/kafka-console-producer.sh --topic cyber-events --bootstrap-server "$BS"
>This is my first cyber event
>This is my second cyber event
```

## Consume messages

```bash
bin/kafka-console-consumer.sh --topic cyber-events --from-beginning --bootstrap-server "$BS"
```

## Delete a topic
```bash
bin/kafka-topics.sh --delete --topic cyber-events --bootstrap-server "$BS"
```

## Consumer groups

## Start a consumer using a named group

```bash
bin/kafka-console-consumer.sh --topic cyber-events --group <group_name> --from-beginning --bootstrap-server "$BS"
```

```bash
bin/kafka-consumer-groups.sh --bootstrap-server "$BS" --list
```
## Describe a consumer group

```bash
bin/kafka-consumer-groups.sh --bootstrap-server "$BS" --describe --group <group_name>
```

Look at these fields:

| Field | Meaning |
| --- | --- |
| GROUP | Consumer group name |
| TOPIC | Topic being consumed |
| PARTITION | Partition assigned to a consumer |
| CURRENT-OFFSET | Consumer group's committed position |
| LOG-END-OFFSET | Current end position of the partition |
| LAG | Difference between end offset and committed offset |

Exercise: Produce additional messages and observe how the group's offsets and lag change.