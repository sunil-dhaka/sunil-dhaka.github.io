---
layout: post
title: Getting Started with Kafka - A Terminal Guide
date: 2023-08-15
categories: ['dev']
tags: ['kafka']
description: Quick guide to running Kafka in your terminal
author: Sunil Dhaka
---

# Demystifying Kafka from the Command Line

Apache Kafka has become the backbone of modern data pipelines, but getting started can feel overwhelming. I recently dove into Kafka and discovered it's not as complex as it seems when you break it down to basics. Here's my terminal-centric guide to getting Kafka up and running on your local machine.

## Setting Up Your Environment

First, you'll need to download Kafka. I prefer using the binary distribution for simplicity:

```bash
# Download the latest Kafka release
wget https://downloads.apache.org/kafka/3.4.0/kafka_2.13-3.4.0.tgz

# Extract the archive
tar -xzf kafka_2.13-3.4.0.tgz
cd kafka_2.13-3.4.0
```

## Starting the Kafka Environment

Kafka requires ZooKeeper for coordination (though newer versions are moving away from this dependency). Let's start both services:

1. First, launch ZooKeeper in one terminal:
```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

2. Then, start the Kafka broker in another terminal:
```bash
bin/kafka-server-start.sh config/server.properties
```

Congratulations! You now have a running Kafka environment on your local machine.

## Creating Your First Topic

In Kafka terminology, a "topic" is a category where messages are published. Let's create one:

```bash
bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

To verify your topic was created:

```bash
bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```

## Sending and Receiving Messages

Now for the fun part—let's send some messages:

1. Launch a producer console in one terminal:
```bash
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```
Type any message and press Enter. Each line you type becomes a separate message.

2. In another terminal, start a consumer to read these messages:
```bash
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```

You should see your messages appearing in the consumer terminal! Play around by sending more messages from the producer—they'll appear in the consumer console almost instantly.

## Shutting Everything Down

When you're done experimenting:

1. Press Ctrl+C to stop the consumer and producer
2. Stop the Kafka broker: `bin/kafka-server-stop.sh`
3. Stop ZooKeeper: `bin/zookeeper-server-stop.sh`

## Next Steps

This is just scratching the surface of what Kafka can do. As you get comfortable with the basics, you might want to explore:

- Creating multi-broker clusters
- Working with consumer groups
- Implementing stream processing with Kafka Streams

Have you worked with Kafka before? What applications are you building with it? I'd love to hear about your experiences!
