---
title: Data in Motion – about being RESTless
transition: fade-out
theme: default
layout: cover
---

# Data in Motion

An introduction to event stream processing

---
layout: center
---


<div style="font-size: 3rem; margin-bottom: 2rem">
How does your data behave?
</div>

<div style="display: flex; justify-content: space-around;">
<v-click>
<div>
<img src="/data-at-rest.png" class="rounded-xl h-60" />
</div>
</v-click>
<v-click>
<div>
<img src="/data-in-motion.png" class="rounded-xl h-60" />
</div>
</v-click>
</div>

---

<Toc maxDepth="2"/>

---
layout: statement
---

## Why does it matter? And why the hype?

<!--
"Data in Motion" is a term coined (probably) by Confluent.

Confluent is a highly regarded company and a major contributor to Apache Kafka. While Apache Kafka is popular in enterprises, this alone doesn't justify the hype.

The real reason is that we strive to **represent reality** in software systems.
-->

---

### Things are now in motion that cannot be undone

<style>
h3 {display: none}
div {background-color: black}
</style>

![ "Things are now in motion that cannot be undone". Gandalf in "Return of the King"](/gandalf-things-in-motion.webp)

<!--
The real world changes ("mutates") constantly, but all **events leading to those changes are fixed in time and space** – they are immutable.

In software, we achieve consistently great results by keeping the **representational gap** low.

For example, a Sales Order on paper can be modeled as a Sales Order Class, implemented with a `SalesOrder` interface, and stored in a `SALES_ORDER_HEADER` and `SALES_ORDER_ITEM` table in a database – or in a `SalesOrders` collection if using an object store.

Historically, we mutated our persistence – because everything else would have been **too costly**

Now that **storage is cheap**, we can finally close this "eventual" representational gap and process those events.
-->


---
layout: statement
---

### Using a Stream Processor unlocks the world of ... event processing

<!--
... and unlocks new capabilities
-->

---
layout: center
---

### Where event stream processing is being used

<style>
h3 {display: none}
</style>

![Companies heavily using event stream processing](/streaming-companies.png)

<!--
### Real-World Examples

- **Alibaba** uses a fork of Apache Flink called Blink to optimize search rankings in real-time.
- **Netflix** uses Apache Flink to power some of the world's largest streaming applications. Not for the packet-streaming, but for *popularity statistics, personalized recommendations, ...* They have implemented Flink on a large scale, with over 10,000 cores managing tens of terabytes of state data.
- **Zalando** uses Apache Flink for real-time business process monitoring and continuous ETL (Extract, Transform, Load) operations.
- **Otto Group**, the world's second-largest online retailer, uses Flink for business intelligence stream processing.
- **Pinterest** runs thousands of so-called "experiments" every day on a platform for real-time experiment analytics that is based on Apache Flink.
- **LinkedIn** for real-time data analysis and personalization.
- **Uber** uses Apache Flink as a processing engine for its streaming platform, for example, to consistently generate features for machine learning.
- **Deutsche Bahn** (currently) uses the Kafka Streams Library to provide insights into trains, schedules, and delays in real time to travelers.
-->

---
layout: center
---

### Real Time Decision Making

<style>
h3 {display: none}
</style>

![Real Time Decision Making](/we-want-realtime.jpg)

<!--
### Real Time Decision Making

In today's fast-paced world, businesses need to make decisions quickly based on up-to-date information. This does not only apply to consumers, but also to enterprise business users ... who **got used to realtime** as consumers

Streaming data architecture enables real-time processing and analysis of data, allowing organizations to react promptly to changing conditions, trends, and events.

... and you never know, ~~if~~ ~~when~~ how quickly your human consumer will be replaced by a much more responsive consuming API.

Examples: Financial trading, track and trace, traffic based routing (of IP packets and/or goods)
-->

---
layout: center
---

### Explosion of data

<style>
h3 {display: none}
</style>
<a href="https://www.statista.com/chart/17727/global-data-creation-forecasts/" target="_blank" title="Infographic: Global Data Creation is About to Explode | Statista">
<img src="https://cdn.statcdn.com/Infographic/images/normal/17727.jpeg" alt="Infographic: Global Data Creation is About to Explode | Statista" class="h-120 shadow"/>
</a>

<!--
### Explosion of data

With the proliferation of connected devices, social media platforms, IoT sensors, and other sources, the volume of data generated every second has **grown exponentially**. 

Traditional batch processing approaches are no longer sufficient to handle this massive influx of data. Streaming data architecture provides a scalable and efficient solution for processing data in real-time as it is generated.
-->

---
layout: statement
---

### Competitive advantage

<!--
Businesses are increasingly recognizing the value of leveraging real-time data to **gain insights**, make informed decisions, and **deliver personalized experiences** to customers. 

Streaming data architecture enables organizations to stay ahead of the competition by offering **timely** and relevant products, services, and recommendations.
-->

---
layout: center
---

### Advanced Analytics

<img alt="Convergence of OLAP and OLTP" src="/oltp-olap.png" class="h-90 rounded-3xl"/>

<style>
h3 {display: none}
</style>

<!--
Streaming data architecture facilitates the implementation of advanced analytics techniques, such as **machine learning, predictive analytics, and anomaly detection**, in real-time. 

This allows organizations to uncover hidden patterns, identify emerging trends, and detect anomalies as they occur, enabling proactive decision-making and risk mitigation.
-->

---

### Alright, we've got Kafka up and running, let's build a REST interface

![https://www.yishizuo.com/dont-be-a-hammer-looking-for-a-nail-3/](/hammer-nail.png)

<!--
there are fundamental differences in those architectures. 

Before looking into streaming architecture components, let's revise a traditional REST application
-->

---
layout: center
---

## Understanding Traditional REST (State) Based Applications

---
layout: center
---

### REST Definition and Principles

<style scoped>
    h2, h3 {display: none}
</style>

![](/DALL-E-world-in-digital.png)

<!--
## Understanding Traditional REST (State) Based Applications – Definition and Principles

Coming back to the "representation" whose gap we tried to close:

For software engineers, understanding the "representation" of real-world entities is crucial because it ensures a consistent and clear way to model, communicate, and manage data throughout the entire software stack. 
When we talk about representation in REST, we refer to how real-world entities (like a customer, order, or product) are modeled and exposed as resources in the API. These representations:

1. **Ensure Consistency**: By having a clear representation of entities, every part of the system (from the database to the client) has a unified understanding of what an entity looks like and how it behaves.
2. **Simplify Communication**: RESTful APIs rely on clear and consistent representations to facilitate communication between different system components. For example, a client application doesn't need to know the internal workings of the server; it only needs to understand the representation of resources to interact with them.
3. **Improve Maintainability**: Consistent representations make it easier to maintain and evolve the system. Changes to the representation (such as adding new fields or changing existing ones) can be managed in a controlled way, minimizing the impact on other system components.
4. **Support Statelessness**: REST's stateless nature means that each request from a client to a server must contain all the information needed to understand and process the request. Clear representations of entities ensure that this information is complete and accurate, allowing the server to process requests without needing to retain session information between requests.

Overall, the representation of real-world entities is a foundational concept in RESTful architecture that supports effective communication, consistency, and maintainability across the software stack.

_Remark: REST APIs typically (but not mandatory) use HTTP methods (GET, POST, PUT, DELETE) to perform CRUD operations._
-->

---
layout: center
---

### REST Request-Response Pattern

![Quarkus rest api implementation components](/quarkus-rest-api.png)

<!--
- **The client initiates** communication by sending a request to the server, which then processes it and returns a response
- There's a lot of transformations and protocol adapters along the way
- Commonly used in **web services** and **APIs**.
- Follows a **synchronous communication** model. Each request waits for a response

Source: https://developers.redhat.com/articles/2022/02/03/build-rest-api-ground-quarkus-20
-->

---

### Which led to microservices

_"generate a picture illustrating client service communication in micro services"_

<img src="/microservices-by-DALL-E.webp" class="h-90"/>

---

### Restful Limitations in Handling Real-Time Data

- Scalability
- Latency
- Statelessness

<!--
- **Scalability**: Processing each consumer's requests increases the load on the server
- **Latency**: Load on the server slows down the consumer
- **Statelessness**: Each request is independent, making real-time updates complex. We all love optimistic locking, don't we?
-->

---
layout: center
---

## Introduction to Streaming Data Architecture

---

### Characteristics of a Streaming Data Architecture


- Continuous data flow
- Real-time processing

<!--
### Streaming Data Architecture Overview

- **Continuous data flow:** Handles data streams that are **constantly generated** by various sources.
- **Real-time processing:** Processes data in real-time or near real-time **as it arrives**.

### Real-time Data Processing Capabilities

- **Event-driven:** Processes events as they occur, leading to timely and relevant responses.
- **Low latency:** Immediate processing allows for quick insights and actions.
- **Scalability:** Can handle large volumes of data from multiple sources.
- **Fault tolerance:** Ensures reliability and accuracy even in case of failures.
-->

---
layout: center
---

![https://thecloudblog.net/post/event-driven-architecture-with-apache-kafka-for-net-developers-part-1-event-producer/](/producer-consumer-stream.png)

<!--
### Principles of Event-Driven Architecture

- **Event producers and consumers:** Entities that generate and respond to events.
- **Asynchronous communication:** Events are processed independently and concurrently.
- **Event streams:** Continuous flow of events that trigger actions or updates, implemented as **topics** in Kafka.
-->

---

### Advantages over REST

- Decoupling
- Scalability
- Real-time processing
- Resilience

<!--
- **Decoupling:** Loose coupling between components enhances flexibility and maintainability.
- **Scalability:** Can handle high volumes of events efficiently (as fast as there is compute)
- **Real-time processing:** Immediate reaction to events provides up-to-date information and responses.
- **Resilience:** Built-in mechanisms for error handling and recovery ensure robustness.
-->

--- 

### REST compared to Event Streaming – What's happening full-stack

<table>
<tr><th></th><th>REST</th><th>Streaming</th></tr>
<v-clicks>
<tr><td>Stored is</td><td>State</td><td>Event</td></tr>
<tr><td>Operation is a</td><td>Mutation</td><td>Map/Reduce/Filter</td></tr>
<tr><td>Transported is</td><td>Consumer optimized state</td><td>Derived event</td></tr>
<tr><td>Reporting is</td><td>OLAP</td><td>Just a view</td></tr>
</v-clicks>
</table>

<!--

-->

---

### REST compared to Event Streaming – Architectural layers

<table>
<tr><th></th><th>REST</th><th>Streaming</th></tr>
<v-clicks>
<tr><td>Storage</td><td>DBMS</td><td>Messaging System</td></tr>
<tr><td>Processing</td><td>Application Server</td><td>Stream Processor</td></tr>
<tr><td>Client API</td><td>HTTP</td><td>Streaming-API</td></tr>
<tr><td>Analytical Persistence</td><td>Data Warehouse</td><td>Data Lake</td></tr>
</v-clicks>
</table>

<!--
(Durable) Messaging Systems **facilitate the transfer of data** between different parts of the system in real-time. Examples: **Kafka, RabbitMQ**.

Stream processing frameworks like Apache Flink and Apache Spark Streaming allow for **complex data processing operations on data streams in real-time**. Examples: **Apache Flink, Apache Spark Streaming, Azure Streaming Analytics**

Streaming APIs **provide full-stack reactive user experiences**. Examples: **Websocket, Server-sent events**

NoSQL databases and data lakes **store vast amounts of real-time data efficiently**. Examples: **HBase (Hadoop), Cassandra**
-->


---

### How does this affect the system

<style>
    .comparison {
        display: flex;
        flex-direction: row;
        justify-content: space-around
    }

    .header {
        font-weight: 600;
        backdrop-filter: contrast(0.5)
    }

    h3 {
        display: none    
    }

    p {margin-bottom: 0.5rem}

</style>


<div class="comparison header">
    <p>
    REST
    </p>
    <p>
    Stream Processing
    </p>
</div>

If we **increase load**, this results in

<div class="comparison">
    <p>
    Slower responses
    </p>
    <p>
    Processing time latency
    </p>
</div>

If we **overload**, this results in

<div class="comparison">
    <p>
    Dropped requests
    </p>
    <p>
    Backpressure
    </p>
</div>

We **scale** the system

<div class="comparison">
    <p>
    Horizontally on the app server, <br/> vertically on the DBMS
    </p>
    <p>
    Horizontally everywhere
    </p>
</div>

**Shit in** results in

<div class="comparison">
    <p>
    Denied service <br/>> (at client or – worse – server side)
    </p>
    <p>
    Dead letters
    </p>
</div>

<!--
## Comparison: Traditional REST vs. *Stateful* Streaming Data Architecture
Compare the performance aspects of traditional REST and streaming data architectures. Discuss how streaming architectures generally offer lower latency by processing data in real-time, whereas REST can experience higher latency due to its request-response model. Explain the concept of backpressure in streaming systems versus dropped requests in REST under high load.

Highlight the scalability differences. Streaming architectures can scale horizontally more efficiently due to their event-driven nature, whereas REST can face challenges as the number of clients and requests increase.

Streaming systems often have built-in fault-tolerance and data recovery mechanisms, whereas REST relies on external solutions for ensuring data integrity and handling failures.
-->

---
layout: statement
---

Ok, you got me. we need event stream processing.

We already got Kafka, we're fine... No?

---
layout: center
---

## Stateless vs. stateful streaming

---
layout: two-cols
---

<style>
.dialogue {
    font-style: italic;
}
</style>

<v-clicks>
<div>Filtering => trivial<img src="/marbles-filtering.png" alt="Filtering" /></div>
<div>Mapping => trivial<img src="/marbles-mapping.png" alt="Mapping" /></div>
</v-clicks>

<template #right>
<v-clicks>
<div>Windowing<img src="/marbles-windowing.png" alt="Windowing" /></div>
<div>Complex Event Processing<img src="/marbles-cep.png" alt="Complex Event Processing" /></div>
<p style="margin-top: 1rem" class="dialogue">But that's also trivial, just some sort of "reduce", no?</p>
<p class="dialogue">Only as long as you are running on a single, reliable node. There’s STATE involved, buddy!</p>
</v-clicks>
</template>
    
---
layout: two-cols
---

### What's the difference

<style>
    h3 {min-height: 6rem}
</style>

**Stateless** processing

- Restricted to One-By-One-processing
- Filtering
- Mapping (data enrichment)

<template #right>
<h3>So what do we need state for?</h3>

**Stateful** computations

- Considers multiple events all together
- Aggregations (over time)
- Business processing which rely on ordering
- Complex Event Processing
</template>

<!--

What about Spring Cloud, Node-Streams, AWS SNS + Lambda? All are
**Stateless**
- All these solutions don't offer built-in-options for state
- State is managed by the DBMS (or Kafka, looking at Kafka-streams)
- State resides on disk
- Simple to scale

**Statefulness**
- Perform computations which consider multiple events
- much harder to scale
-->

---
layout: image-right
image: /flink-squirrel-stream-hazelnut.webp
---
<div class="align-middle">
<h2>Stream processing with Apache Flink</h2>
</div>

---
layout: center
---

### Integration of Flink into the streaming landscape

![The Flink-Kafka-ecosystem](/flink-kafka-ecosystem.png)

---


### Use cases

- Event Driven Applications
- Streaming ETL pipelines
- Real-time analytics

<!-- https://www.infoworld.com/article/2336241/3-dynamic-use-cases-for-apache-flink-and-stream-processing.html -->

<!--
- **Event Driven Applications**
  trigger actions on events, including complex event processing (CEP, which includes identifying patterns in the order of events)
- **Realtime analytics**
  Provide real-time insights into aggregated events
- **Streaming pipelines**
  Continuously ingest data into warehouses for further (state-oriented) analysis
-->

---

#### Event Driven Applications

![Event driven Application Architecture](/spaf_0105.png)

<ImageSource work="spaf" />

<!--

Event-driven applications are stateful streaming applications that ingest event streams and process the events with application-specific business logic. Depending on the business logic, an event-driven application can trigger actions such as sending an alert or an email or write events to an outgoing event stream to be consumed by another event-driven application.

Event-driven applications are an evolution of microservices. They communicate via event logs instead of REST calls and hold application data as local state instead of writing it to and reading it from an external datastore

_Source: Stream Processing with Apache Flink by Fabian Hueske_
-->

---

#### Streaming ETL pipelines

<v-click hide>
<img src="/spaf_0107.png" class="h-70 absolute">
<ImageSource work="spaf" />
</v-click>


<v-click at="1">
<img src="/spaf_0107_only_speed.png" class="h-70">
<ImageSource work="spaf" />
</v-click>


<!--

Traditional Lambda-Architectures have significant drawbacks:

- it requires two semantically equivalent implementations of the application logic for two separate processing systems with different APIs. 
- the results computed by the stream processor are only approximate (might be overridden as the Batch-results are available).

The Third-Gen-Stream-Processors** addressed the dependency of results on the timing and order of arriving events**. In combination with **exactly-once failure semantics**, systems of this generation are the first open source stream processors capable of computing consistent and accurate results.

-->

---
layout: center
---

#### Real-time analytics (aka. "Streaming Analytics")

![Streaming Analytics Architecture](/spaf_0106.png)
<ImageSource work="spaf" />
<!--

Instead of waiting to be periodically triggered, a streaming analytics application continuously ingests streams of events and updates its result by incorporating the latest events with low latency. This is similar to the maintenance techniques database systems use to update materialized views.

_Source: Stream Processing with Apache Flink by Fabian Hueske_
-->


---

### Which problems does Flink solve

<div class="hypothesis">
Apache Flink solves a lot of issues you might not even be aware of you've actually got them...
</div>

- Event-Time
- Low latency
- Consistency guarantees
- Data locality and Scalability
- A Stream-First-Application programming model

<!--
Apache Flink is a third-generation distributed stream processor with a competitive feature set. It provides **accurate stream processing with high throughput and low latency at scale**. In particular, the following features make Flink stand out:

**Event-time semantics provide consistent and accurate results despite out-of-order events**. Processing-time semantics can be used for applications with very low latency requirements.

**Exactly-once** means that not only will there be no event loss, but also updates on the internal state will be applied exactly once for each event. In essence, exactly-once guarantees mean that our application will provide the correct result, as though a failure never happened.

**Millisecond latencies** while processing millions of events per second. Flink applications can be scaled to run on thousands of cores.

**Layered APIs** with varying tradeoffs for expressiveness and ease of use. 

**Connectors** to the most commonly used storage systems such as Apache Kafka, Apache Cassandra, Elasticsearch, JDBC, Kinesis, and (distributed) filesystems such as HDFS and S3.

Ability to run streaming applications **24/7 with very little downtime due to its highly available setup** (no single point of failure), tight integration with Kubernetes, YARN, and Apache Mesos, fast recovery from failures, and the ability to dynamically scale jobs.

Ability to update the application code of jobs and **migrate** jobs to different Flink clusters **without losing the state** of the application.

_Source: Stream Processing with Apache Flink by Fabian Hueske_

-->

---
layout: statement
---

#### Event time and watermarks

<style>
    h4 {display: none}
</style>


Event time is the time when an event in the stream actually happened. Event time is based on a timestamp that is attached to the events of the stream. 

<!-- 
Timestamps usually exist inside the event data before they enter the processing pipeline (e.g., the event creation time)
-->

---
layout: center
---

![A watermarked stream](/spaf_0308.png)
<ImageSource work="spaf" />

<!--
Watermarks are determined (extracted) from records.

**When** they will be emitted, can be configured / implemented.
-->

---
layout: statement
---

#### Low Latency

Did somebody say "Real-time"?

---

My mom says
<div class="hypothesis">"Real-time is when a system is faster than I can react"</div>

<v-clicks>

As software developers, we can say

**Real-time** is to process Data as soon as it's emitted

**Latency** defines how long the consumer has to wait until the processing has terminated

**Tolerated Latency** usually depends on the consumer's ability to react.
</v-clicks>

<!--
Disclaimer: My mom didn't actually say that. I derived it from her words when she told me to clean up my room "NOW" when I was a child.

As architects, we should wish for analysts to specify tolerated latency prior to designing a system.
Unfortunately, this decision is usually outside of the system boundary but part of the context – and this context may vary.
-->


---
layout: two-cols
---

#### Data locality

<img alt="Memory Hierarchy and read access latency" src="/memory-hierarchy.png" class="pd-20 h-50"/>
<ImageSource url="https://cs.brown.edu/courses/csci1310/2020/assign/labs/lab4.html"/>

<template #right>
<v-click>
<h4>Co-Location of memory and compute in Flink</h4>

<img alt="In Flink, data of a task is located at the same node where operations on it are executed" src="/spaf_0104.png" class="pd-20 h-60"/>
  <ImageSource work="spaf" />
</v-click>
</template>

<!--
Having data local at the same node in-memory will speed up our **read times by factor 1000**
This is important, since in **one-by-one-processing**, we constantly access data.

"Local" means that the physical memory in which the data resides **on the same compute node** where the physical CPU hosts the computation over this data.

Stateless architectures (be it REST or stateless streaming) mostly load data from a remote (on another node) location via network – and even a Redis-Cache via network is about 5ms – slow.
-->

---
layout: statement
---

#### Scalability

Alright, let's keep data local. But what do we do if we scale horizontally?

<!--
Scaling horizontally means **adding new nodes** to which compute-demands/traffic is routed.

When keeping state local, this means that the scaling process needs to be aware of where data is being processed.
-->

---
layout: two-cols
---

##### Keyed streams and keyed state

<img alt="Figure 3-12. Tasks with keyed state" src="/spaf_0312.png" class="m-10 h-80" />
<ImageSource work="spaf" />
<template #right>
<v-click>
<h5>Scaling stateful (keyed) operators</h5>
<img alt="Figure 3-13. Scaling an operator with keyed state out and in" src="/spaf_0313.png"  class="m-10 h-80"/>
<ImageSource work="spaf" />
</v-click>
</template>
<!--
Flink allows developers to partition a stream by a logical key. 

All operations on a keyed stream will respect data locality: The operator instance of a Flink computational graph will get all items of the keyed stream which has the same key.
-->

---

#### Consistency guarantees

- Flink provides "Exactly Once" **processing guarantees** within the job graph
- **Some sources and sinks** support this out-of-the-box too
- Flink's **checkpointing mechanism** ensures guarantees in case of failures

<!--
Flink is a distributed data processing system, and as such, has to deal with failures such as **killed processes, failing machines, and interrupted network connections**. Since tasks maintain their state locally, Flink has to ensure that this **state is not lost and remains consistent** in case of a failure.

Flink’s recovery mechanism is based on consistent checkpoints of application state. A consistent checkpoint of a stateful streaming application is a copy of the state of each of its tasks at a point when all tasks have processed exactly the same input.
-->

---
layout: two-cols
---

##### Naive Checkpointing

1. <span v-mark="{ color: 'red', type: 'circle' }">Pause</span> the ingestion of all input streams.
2. Wait for all in-flight data to be completely processed, meaning all tasks have processed all their input data.
3. Take a checkpoint by copying the state of each task to a remote, persistent storage. The checkpoint is complete when all tasks have finished their copies.
4. Resume the ingestion of all streams.
 
<template #right>
<v-click>
<h5>Flink's checkpointing</h5>

1. Inject Checkpoint Barriers into stream
2. Job manager initiates checkpoint
3. Each operator externalizes its state
4. Once all operators acknowledged, the checkpoint is consistent

</v-click>

<!--
##### Flink’s Checkpointing Algorithm

Flink’s recovery mechanism is based on consistent application checkpoints. The naive approach to taking a checkpoint from a streaming application—to **pause, checkpoint, and resume the application—is not practical for applications** that have even moderate latency requirements due to its “stop-the-world” behavior. 

Instead, Flink implements checkpointing based on the Chandy–Lamport algorithm for **distributed snapshots. The algorithm does not pause the complete application but decouples checkpointing from processing**, so that some tasks continue processing while others persist their state. In the following, we explain how this algorithm works.

Flink’s checkpointing algorithm uses a special type of record called a checkpoint barrier. Similar to watermarks, checkpoint barriers are injected by source operators into the regular stream of records and cannot overtake or be passed by other records. A checkpoint barrier carries a checkpoint ID to identify the checkpoint it belongs to and logically splits a stream into two parts. All state modifications due to records that precede a barrier are included in the barrier’s checkpoint and all modifications due to records that follow the barrier are included in a later checkpoint.

The same mechanism can also be used for **savepoints**, which allow to externalize state at defined points in time (e.g., prior to an application upgrade)
-->
</template>


---

### Which problems does Flink produce

<div class="hypothesis">
Apache Flink solves a lot of issues you might not even be aware of you've actually got them...

... but it also produces some you even more likely not thought about either
</div>

- Complexity: Learning curve
- Monitoring
- State backend
- Job manager / task manager communication
- Running and scaling the infrastructure (Choose your ~~poison~~ operations model)

<!--

Stream processing requires a **different mental model** compared to state-oriented application programming models. 

Also, there's an **"event" in "eventual consistency"**. While Flink will make sure, that in the end, everything is processed consistently, there might be steps in between where some parts have been processed, while others are still pending. This may require explanations to the user on the UI.

Observing, how a continuously running application behaves requires appropriate metrics. They deviate from what's common to observe for restful applications.

The way, job manager and task manager interact, also with the state-backend, is not easy to understand. It's deeply integrated into the tooling.
-->

---

### How to respond to those challenges

<v-clicks>

- Acknowledge it's a tricky problem
- Start small in functional scope, but realistic wrt volume 
- Include the other elements of the streaming architecture (Schema Registry!)
- Set up custom metrics from the beginning
- Start with a Managed Service (AWS or Azure)

</v-clicks>
<!--

[click]
It's a tricky problem, but **it's a problem Flink is made for**. 

Still, a tricky problem. Allow yourself and your developers time to learn!

Flink provides multiple APIs for different use cases, has great documentation, but is just a lot to learn. Java Streaming API, Python, TableAPI, SQL – The more abstract, the more to learn

[click]
A major benefit of Flink is **performance and scalability**. But this only works when employed properly. Pick use cases which stress your system right from the beginning.

[click]
As pictured earlier, Flink ~~is~~ can be the central piece of your streaming architecture. But don't forget the other pieces and how to bring them together. A **schema registry** might save you from tedious serialization issues down the stream, if you implement them from beginning.

[click]
Flink has an **awesome metrics system** which export very detailed information about the state of processing (offset lags, backpressure, parallelism)
The metrics system is also easily extensible: Write your own custom metrics to observe how your stream is doing.

[click]
Job manager, Task manager, State-Backend does not need to be understood in detail, as Flink abstracts all this provisioning of compute.
This changes drastically, if you want to operate Flink on your own though. Then, you need to completely dive in to make it run elastically on your e.g., Kubernetes-Cluster.

There are operators for Kubernetes, Apache Mesos and bare metal installations, but safest is to **start with a managed service**. It might not suit your particular use case and could be optimized, but configuring them properly needs some experience.
-->

---
layout: statement
---

### So (why) should I give Flink a try?

---
layout: statement
---

“you’re either using a framework, or you’re building your own framework.”

<div>
The React Native team explaining their recommendation to build React Native using Expo
</div>
<!--

-->


---
layout: center
---

![Flink is the hammer for event stream processing](/DALL·E-squirrel-with-hammer.webp)

<!--

**Because Apache Flink is made for this**

There are challenges to be solved, but there's always a Flink-native solution to it. Because it's a **mature** and **battle proven** technology.
-->

---
layout: two-cols
---

<img alt="Flink Logo" src="/flink_squirrel_500.png" class="h-90" />
https://flink.apache.org/

<template #right>

<img alt="Stream Processing with Apache Flink by Fabian Hueske and Vasiliki Kalavri" src="/spaf_cover.png" class="h-90" />

</template>
