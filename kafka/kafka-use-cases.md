# Kafka use-cases

## Problem



Microservice architecture requires communication with them.

For example service A requests to service B, and B requests to c and D.

When communications are synchronous  A has to wait for all these requests.

And what if A would become unavailable?

## Activity tracking



The original use case is user activity tracking. Users interact with frontend application and produce a lot of events which consumes by backend layers and generate reports, feed machine learning, update search results, etc.

## Messaging



Messaging between applications, and not concern about consuming.

## Metrics and Logging



We have multiple applications producing the same type of messages. Log messages can be published in the same way, and can be routed to dedicated log search systems like Elastisearch or security analysis applications. Another added benefit of Kafka is that when the destination system needs to change (e.g., it’s time to update the log storage system), there is no need to alter the frontend applications or the means of aggregation.

## Commit log



Some changes can be published to Kafka and other applications can easily monitor this stream to receive live updates when they happen.

It can be used for database replication.

Consolidating changes between different applications into a single database view.

## Stream processing



While almost all usage of Kafka can be thought of as stream processing, the term is typically used to refer to applications that provide similar functionality to map/reduce processing in Hadoop.

## My experience of using Kafka



1. Service A sends files to B to check for antivirus and doesn't wait for the result. The result can be consumed through Kafka.
2. Steps-engine microframework. Service produces an operation that goes through several steps, during each step operation writes to Kafka, and even if the service goes down, after reload it goes from the needed step. Steps should be idempotent.
