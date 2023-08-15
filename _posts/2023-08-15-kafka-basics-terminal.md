---
layout: post
title: getting started with kafka 
date: 2023-08-15
categories: [kafka]
tags: [cli, kafka]
description: basics of kafka, and how to work with kafka in cli
author: Sunil Dhaka
---


## Getting Started with Kafka on macOS CLI

Introduction:
Apache Kafka is a distributed event streaming platform that is widely used for building real-time data pipelines and streaming applications. In this guide, we'll walk you through the process of installing and working with Kafka on macOS using the command line interface (CLI). We'll cover installation, starting Kafka and ZooKeeper, and provide some practical examples to help you get started.

Prerequisites:
1. macOS environment.
2. Homebrew package manager installed.

Installing Kafka:
Kafka can be easily installed using Homebrew. Open your terminal and follow these steps:

1. Open your terminal.
2. Run the following command to install Kafka using Homebrew:
   ```
   brew install kafka
   ```

This will install Kafka and ZooKeeper on your system.

Starting Kafka and ZooKeeper:
Kafka requires ZooKeeper for distributed coordination. Let's start both Kafka and ZooKeeper:

1. Navigate to the Kafka installation directory (usually located at `/opt/homebrew/etc/kafka`):
   ```
   cd /opt/homebrew/etc/kafka
   ```

2. Start ZooKeeper:
   ```
   zookeeper-server-start zookeeper.properties
   ```

3. Open a new terminal tab/window and navigate to the same Kafka directory.

4. Start Kafka server:
   ```
   kafka-server-start server.properties
   ```

Creating a Kafka Topic:
Let's create a Kafka topic where we'll publish and consume messages.

1. In a new terminal tab/window, navigate to the Kafka directory.

2. Create a new topic named "test-topic":
   ```
   kafka-topics --create --topic test-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
   ```

Publishing Messages:
In this example, we'll publish messages to the "test-topic".

1. Open a terminal tab/window and navigate to the Kafka directory.

2. Start a producer to publish messages:
   ```
   kafka-console-producer --topic test-topic --bootstrap-server localhost:9092
   ```

3. Start typing messages and press Enter to publish them to the topic.

Consuming Messages:
Let's consume the messages we published earlier.

1. Open a terminal tab/window and navigate to the Kafka directory.

2. Start a consumer to receive messages:
   ```
   kafka-console-consumer --topic test-topic --bootstrap-server localhost:9092 --from-beginning
   ```

You will see the messages you published being displayed in the consumer terminal.

Conclusion:
Congratulations! You have successfully installed Apache Kafka on macOS using the CLI, created a topic, and published and consumed messages. This is just the beginning of what Kafka can do. You can explore more advanced configurations, multiple topics, and integrate Kafka into your applications for real-time data streaming and processing.

Kafka provides a powerful foundation for building scalable, reliable, and high-performance data pipelines. As you delve deeper, you'll discover its potential for building sophisticated event-driven applications and managing real-time data streams.
