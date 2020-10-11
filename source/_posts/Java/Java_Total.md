---
title: Java Note
date: 2020-10-11 18:00:26
tags:
    - java
    - note_total
category: 
    - java
---


## KafkaListener - chỉ định vị trí offset + partition
```java
 @KafkaListener(
            topics = "abc.ProductLogs111",
            groupId = "tmp-remove-whenever-001",
            concurrency = "1",
            topicPartitions = @TopicPartition(topic = "abc.ProductLogs",
                    partitionOffsets = {
                            @PartitionOffset(partition = "2", initialOffset = "2049"),
                            @PartitionOffset(partition = "0", initialOffset = "2325"),
                            @PartitionOffset(partition = "1", initialOffset = "2049"),
                    })
    )
    public void listen(ConsumerRecord<String, String> record) throws InvocationTargetException, NoSuchMethodException, InstantiationException, IllegalAccessException, IOException {
        try {
            System.out.println("topic: " + record.topic());
            System.out.println("partition: " + record.partition());
            System.out.println("offset: " + record.offset());
            System.out.println("value: " +record.value());
            System.out.println("timeStamp: " +record.timestamp());
        } catch (Exception e) {
            //logger.error("[Topic] " + record.topic() + " [Offset] " + record.offset() + " [Partition] " + record.partition() + " [Exception] ", e);
            logger.error("Kafka consumer failed: ", e);
            Sentry.capture(e);
            throw e;
        }
    }
```