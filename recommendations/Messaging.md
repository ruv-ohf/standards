# Messaging

## Adopt

  - [Amazon SQS](http://aws.amazon.com/sqs/)

    For worker queues, SQS has proven to be an effective technology and is in use in several projects at Gilt.

## Trial

  - [Amazon Kinesis](http://aws.amazon.com/kinesis/)
  
    Potential replacement for Kafka. Note, there are a number of limits currently in Kinesis.

  - [Amazon SNS](http://aws.amazon.com/sns/)
    
    Potential replacement for rabbit, where messages to SNS can be hooked up to multiple SQS topics for consumption.

## Hold

  - [Kafka](http://kafka.apache.org)
  - [RabbitMQ](https://www.rabbitmq.com)
  
    We've opted to pause on maintaining our own messaging infrastructure in favor of already managed solutions.
