```markdown
---
title: Data in Motion – about being RESTless
transition: fade-out
theme: default
layout: cover
hideInToc: false
---

# Data in Motion

An introduction to event stream processing

<!--
**Welcome**

I'm going to talk about event stream processing.

I make a distinction from "streaming," which is usually synonymous with transmitting packets of data (e.g., video streams).

In this session, we'll talk about data—business data—and what happens when it starts moving.
-->

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

<!--
When we describe features of `data` in business applications, we usually discuss **structure.**
We also look at the volume and frequency of changes.
However, we don't often discuss the **mutability** of the things this data describes.

[click]
There are forms of data that are residual. This includes master data and business data like sales orders or documents. Even a virtual simulation can be considered restful. It is created, some perform updates, and then it's mostly read.

[click]
A change in perspective can highlight another aspect of data: How does this data change? If we need to provide an audit trail for master data or collaborate on documents, it makes sense to focus on the **event that triggers a change** as the primary subject of an application.
-->

---
hideInToc: true
---

<style>
li {
  margin-top: 2rem
}
</style>

## ToC

<Toc maxDepth="2"  minDepth="2"/>

<!--
We'll dive into this now, but to understand what's **different** in streaming architectures, we first need to look at what's **common in state-oriented applications.**
-->

---
layout: statement
---

## Why does it matter? And why the hype?

<!--
"Data in Motion" is a term likely coined by Confluent.

Confluent is a respected company and a major contributor to Apache Kafka. While Apache Kafka is popular in enterprises, this alone doesn't justify the hype.

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
The real world changes ("mutates") constantly, but all **events leading to those changes are fixed in time and space**—they are immutable.

In software, we achieve great results by minimizing the **representational gap**.

For example, a Sales Order on paper can be modeled as a Sales Order Class, implemented with a `SalesOrder` interface, and stored in a `SALES_ORDER_HEADER` and `SALES_ORDER_ITEM` table in a database—or in a `SalesOrders` collection if using an object store.

Historically, we modified our persistence because everything else was **too costly.**

Now that **storage is cheap**, we can close this "eventual" representational gap and process those events.
-->

---
layout: statement
---

### Using a Stream Processor unlocks the world of... event processing

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

- **Alibaba** uses a fork of Apache Flink called Blink to optimize search rankings in real time.
- **Netflix** uses Apache Flink to power some of the world's largest streaming applications—not for packet-streaming, but for *popularity statistics, personalized recommendations,* and more. They have implemented Flink on a large scale, with over 10,000 cores managing tens of terabytes of state data.
- **Zalando** uses Apache Flink for real-time business process monitoring and continuous ETL (Extract, Transform, Load) operations.
- **Otto Group**, the world's second-largest online retailer, uses Flink for business intelligence stream processing.
- **Pinterest** runs thousands of so-called "experiments" every day on a platform for real-time experiment analytics based on Apache Flink.
- **LinkedIn** for real-time data analysis and personalization.
- **Uber** uses Apache Flink as a processing engine for its streaming platform, consistently generating features for machine learning.
- **Deutsche Bahn** (currently) uses the Kafka Streams Library to provide insights into trains, schedules, and delays in real time to travelers.
-->

---
layout: center
---

### Real-Time Decision Making

<style>
h3 {display: none}
</style>

![Real-Time Decision Making](/we-want-realtime.jpg)

<!--
### Real-Time Decision Making

In today's fast-paced world, businesses need to make decisions quickly based on up-to-date information. This applies not only to consumers but also to enterprise business users... who have **got used to real-time** as consumers.

Streaming data architecture enables real-time processing and analysis of data, allowing organizations to react promptly to changing conditions, trends, and events.

... and you never know how quickly your **human consumer will be replaced by a much more responsive consuming API**.

Examples: Financial trading, track and trace, traffic-based routing (of IP packets and/or goods).
-->

---
layout: center
---

### Explosion of Data

<style>
h3 {display: none}
</style>
<a href="https://www.statista.com/chart/17727/global-data-creation-forecasts/" target="_blank" title="Infographic: Global Data Creation is About to Explode | Statista">
<img src="https://cdn.statcdn.com/Infographic/images/normal/17727.jpeg" alt="Infographic: Global Data Creation is About to Explode | Statista" class="h-120 shadow"/>
</a>

<!--
### Explosion of Data

With the growth of connected devices, social media platforms, IoT sensors, and other sources, the volume of data generated every second has **grown exponentially.**

Traditional batch processing methods can no longer handle this massive influx of data. Streaming data architecture provides a scalable and efficient solution for processing data in real time as it is generated.
-->

---
layout: center
---

### Competitive Advantage

<div class="relative">
<img src="/DALL·E-disappointed-man.webp" class="h-100" />

<v-clicks>
<img src="/DALL·E-delighted-man.webp" class="h-100 absolute top-0 left-0"/>
</v-clicks>
</div>

<!--
Businesses increasingly recognize the value of leveraging real-time data to **gain insights**, make informed decisions, and **deliver personalized experiences** to customers.

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
Streaming data architecture facilitates advanced analytics techniques, such as **machine learning, predictive analytics, and anomaly detection,** in real time.

This allows organizations to uncover hidden patterns, identify emerging trends, and detect anomalies as they occur, enabling proactive decision-making and risk mitigation.

This leads to a **convergence of analytical and transactional processing.**
-->

---

### Alright, we've got Kafka up and running, let's build a REST interface

![https://www.yishizuo.com/dont-be-a-hammer-looking-for-a-nail-3/](/hammer-nail.png)

<!--
There are fundamental differences in those architectures.

Before looking into streaming architecture components, let's **revise a traditional REST application.**
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

Let's return to the "representation" whose gap we tried to close:

For software engineers, understanding the "representation" of real-world entities is crucial because it ensures a consistent and clear way to model, communicate, and manage data throughout the software stack.
When we talk about representation in REST, we refer to how real-world entities (like a customer, order, or product) are modeled and exposed as resources in the API. These representations:

1. **Ensure Consistency**: A clear representation of entities means every part of the system (from database to client) has a unified understanding of what an entity looks like and how it behaves.
2. **Simplify Communication**: RESTful APIs rely on clear and consistent representations for communication between different system components. For example, a client application doesn't need to know the internal workings of the server; it only needs to understand the representation of resources to interact with them.
3. **Improve Maintainability**: Consistent representations make it easier to maintain and evolve the system. Changes to the representation (such as adding new fields or changing existing ones) can be managed in a controlled way, minimizing the impact on other system components.
4. **Support Statelessness**: REST's stateless nature means each request from a client to a server must include all the information needed to understand and process the request. Clear representations ensure this information is complete and accurate, allowing the server to process requests without retaining session information between requests.

Overall, representing real-world entities is a foundational concept in RESTful architecture that supports effective communication, consistency, and maintainability across the software stack.

_Remark: REST APIs typically (but not necessarily) use HTTP methods (GET, POST, PUT, DELETE) to perform CRUD operations._
-->

---
layout: center
---

### REST Request-Response Pattern

![Quarkus REST API implementation components](/quarkus-rest-api.png)

<!--
- **The client starts** communication by sending a request to the server, which then processes it and returns a response.
- There are many transformations and protocol adapters along the way.
- Commonly used in **web services** and **APIs**.
- Follows a **synchronous communication** model. Each request waits for a response.

Source: https://developers.redhat.com/articles/2022/02/03/build-rest-api-ground-quarkus-20
-->

---

### Which Led to Microservices

_"Generate a picture illustrating client-service communication in microservices."_

<img src="/microservices-by-DALL-E.webp" class="h-90"/>

---
layout: two-cols-header
---

<style>
.two-cols-header{
  grid-template-rows: auto !important
}
</style>

### Restful ~~Limitations~~ Challenges in Handling Real-Time Data

::left::

- Managing State (local data is fast)
- Latency (how long does it take)
- Scalability (reacting to higher demand)

::right::

<v-click>
<img src="/triangle-of-rest.svg" />
</v-click>

<!--
- **Scalability**: Processing each consumer's requests increases the load on the server. **The more clients, the more load** (not necessarily more data!)
- **Latency**: Working with request queues: Load on the server slows down the consumer (or causes lost information) (see [this wonderful animation](https://encore.dev/blog/queueing)).
- **Statelessness**: Each **request is independent**, making real-time updates complex. We all love optimistic locking, don't we?
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
- **Real-time processing:** Processes data in real time or near real time **as it arrives**.

### Real-time Data Processing Capabilities

- **Event-driven:** Processes events as they occur, leading to timely and relevant responses.
- **Low latency:** Immediate processing allows for quick insights and actions.
- **Scalability:** Can handle large (and varying!) volumes of data from multiple sources.
- **Fault tolerance:** Ensures reliability and accuracy even in case of failures.
-->

---
layout: center
---
### Principles of Event-Driven Architecture

![https://thecloudblog.net/post/event-driven-architecture-with-apache-kafka-for-net-developers-part-1-event-producer/](/producer-consumer-stream.png)

<!--
### Principles of Event-Driven Architecture

- **Event producers and consumers:** Entities that generate and respond to events.
- **Asynchronous communication:** Events are processed independently and concurrently.
- **Event streams:** Continuous flow of events that trigger actions or updates, implemented as **topics** in Kafka.
-->

---
layout: two-cols-header
---

### Naive Features of Streaming

::left::

**Easier**

- Decoupling
- Scalability
- Real-time processing
- Resilience

::right::

<v-click>

**Trickier**

- Providing a "current image"

</v-click>

<!--
A **competitive streaming architecture** provides

- **Decoupling:** Loose coupling between components enhances flexibility and maintainability.
- **Scalability:** Can handle high volumes of events efficiently (as fast as there is compute).
- **Real-time processing:** Immediate reaction to events provides up-to-date information and responses.
- **Resilience:** Built-in mechanisms for error handling and recovery ensure robustness (it's part of the "streaming API," while it has to be built into the server in REST).

[click]

Providing the current state needs **event sourcing** or a replay of all events from the latest snapshot.

-->

---

### REST Compared to Event Streaming – What's Happening Full-Stack

<table>
<tr><th></th><th>REST</th><th>Streaming</th></tr>
<v-clicks>
<tr><td>Stored is</td><td>State</td><td>Event</td></tr>
<tr><td>Operation is a</td><td>Mutation</td><td>Map/Reduce/Filter</td></tr>
<tr><td>Transported is</td><td>Consumer optimized state</td><td>Derived event</td></tr>
<tr><td>Reporting is</td><td>OLAP</td><td>Just a view</td></tr>
</v-clicks>
</table>

---

### REST Compared to Event Streaming – Architectural Layers

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
(Durable) Messaging Systems **facilitate the transfer of data** between different parts of the system in real time. Examples: **Kafka, RabbitMQ**.

Stream processing frameworks like Apache Flink and Apache Spark Streaming allow for **complex data processing operations on data streams in real-time.** Examples: **Apache Flink, Apache Spark Streaming, Azure Streaming Analytics.**

Streaming APIs **provide full-stack reactive user experiences.** Examples: **WebSocket, Server-sent events.**

NoSQL databases and data lakes **store vast amounts of real-time data efficiently.** Examples: **HBase (Hadoop), Cassandra.**
-->

---

### How Does This Affect the System

<style>
    .comparison {
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }

    .header {
        font-weight: 600;
        backdrop-filter: contrast(0.5);
    }

    h3 {
        display: none;
    }

    p {margin-bottom: 0.5rem;}

</style>

<div class="comparison header">
    <p>
    REST
    </p>
    <p>
    Stream Processing
    </p>
</div>

<v-click>

If we **increase load**, this results in

<div class="comparison">
    <p>
    Slower responses
    </p>
    <p>
    Processing time latency
    </p>
</div>
</v-click>
<v-click>

If we **overload**, this results in

<div class="comparison">
    <p>
    Dropped requests
    </p>
    <p>
    Backpressure
    </p>
</div>
</v-click>
<v-click>

We **scale** the system

<div class="comparison">
    <p>
    Horizontally on the app server,<br/> vertically on the DBMS
    </p>
    <p>
    Horizontally everywhere
    </p>
</div>
</v-click>
<v-click>

**Bad input** results in

<div class="comparison">
    <p>
    Denied service<br/>(at client or—worse—server side)
    </p>
    <p>
    Dead letters
    </p>
</div>
</v-click>

<!--
How do these features influence runtime?

=> There are pros and cons, but given the need to process a **continuous flow of immutable data**, using a streaming architecture makes sense.
-->

---
layout: statement
---

Okay, you got me; we need event stream processing.

We already have Kafka; we're fine... right?

<!--
As you might guess: **it depends.**
-->

---
layout: center
---

## Stateless vs. Stateful Streaming

<!--
Let's examine the operations we perform when developing streaming applications: Filter, Map, and Reduce.
-->

---
layout: two-cols
---

<style>
.dialogue {
    font-style: italic;
}
</style>

<v-clicks>
<div>Filtering => trivial <img src="/marbles-filtering.png" alt="Filtering" /></div>
<div>Mapping => trivial <img src="/marbles-mapping.png" alt="Mapping" /></div>
</v-clicks>

<template #right>
<v-clicks>

<div>Windowing<img src="/marbles-windowing.png" alt="Windowing" /></div>
<div>Complex Event Processing<img src="/marbles-cep.png" alt="Complex Event Processing" /></div>
<p style="margin-top: 1rem" class="dialogue">But that's also trivial, just some sort of "reduce," right?</p>
<p class="dialogue">Only while running on a single, reliable node. There’s STATE involved, buddy!</p>
</v-clicks>
</template>

<!--
[click]
Filtering => operates on a single event => trivial.

[click]
Mapping => operates on a single event => trivial.

[click]
Windowing => we need to consider multiple events in a time or count window.

[click]
CEP => The order matters; multiple events must be considered.

[click]
Whenever we need information about more than the current event, we need state.

[click]
That's not a bad thing—it just makes it more complex to scale (or achieve zero downtime).
-->

---
layout: two-cols
---

### What's the Difference?

<style>
    h3 {min-height: 6rem;}
</style>

**Stateless** Processing

- Restricted to one-by-one processing
- Filtering
- Mapping (data enrichment)

<template #right>

<h3>So what do we need state for?</h3>

**Stateful** Computations

- Considers multiple events together
- Aggregations (over time)
- Business processing that relies on ordering
- Complex Event Processing
</template>

<!--
What about Spring Cloud, Node-Streams, AWS SNS + Lambda? All are **stateless**.
- These solutions don't offer built-in options for state.
- State is managed by the DBMS (or Kafka, looking at Kafka Streams).
- State resides on disk.
- Simple to scale.

**Statefulness**
- Performs computations that consider multiple events.
- Much harder to scale.
-->

---
layout: center
---

### The Streaming Triangle of Death

<div class="relative">

<img src="/triangle-of-streaming.svg" class="h-100" />

<v-clicks>
<img src="/triangle-of-streaming-skull.svg" class="h-100 absolute top-0 left-0"/>
<img src="/triangle-of-streaming-squirrel.svg" class="h-100 absolute top-0 left-0"/>
</v-clicks>

</div>

<!--
Recalling the unhappy triangle of REST, we can also see a similar conflict sphere for streaming.

[click]
The major difference compared to the RESTful dilemma is the consequences (like backpressure on the server side versus dropped requests). Satisfying all dimensions is challenging!

[click]
This is where Apache Flink positions itself. The features it provides will be discussed in the next slides.
-->
---
layout: image-right
image: /flink-squirrel-stream-hazelnut.webp
---

<div class="align-middle">
<h2>Stream Processing with Apache Flink</h2>
</div>

---
layout: center
---

### Integration of Flink into the Streaming Landscape

![The Flink-Kafka-Ecosystem](/flink-kafka-ecosystem.png)

---

### Use Cases

- Event-Driven Applications
- Streaming ETL Pipelines
- Real-Time Analytics

<!-- https://www.infoworld.com/article/2336241/3-dynamic-use-cases-for-apache-flink-and-stream-processing.html -->

<!--
- **Event-Driven Applications** trigger actions based on events, including complex event processing (CEP, which identifies patterns in the order of events).
- **Real-Time Analytics** provide insights into aggregated events.
- **Streaming Pipelines** continuously ingest data into warehouses for further (state-oriented) analysis.
-->

---

#### Event-Driven Applications

![Event-Driven Application Architecture](/spaf_0105.png)

<ImageSource work="spaf" />

<!--
Event-driven applications are stateful streaming applications that ingest event streams and process the events with application-specific business logic. Depending on the business logic, an event-driven application can trigger actions, such as sending an alert or an email or writing events to an outgoing event stream to be consumed by another event-driven application.

Event-driven applications are an evolution of microservices. They communicate via event logs instead of REST calls and hold application data as local state instead of writing it to and reading it from an external datastore.

_Source: Stream Processing with Apache Flink by Fabian Hueske_
-->

---

#### Streaming ETL Pipelines

<v-click hide>
<img src="/spaf_0107.png" class="h-70 absolute">
<ImageSource work="spaf" />
</v-click>

<v-click at="1">
<img src="/spaf_0107_only_speed.png" class="h-70">
<ImageSource work="spaf" />
</v-click>

<!--
Traditional Lambda Architectures have significant drawbacks:

- They require two semantically equivalent implementations of the application logic for two separate processing systems with different APIs.
- The results computed by the stream processor are only approximate (they might be overridden as the Batch results become available).

The Third-Gen Stream Processors addressed the dependency of results on the timing and order of arriving events. In combination with **exactly once** failure semantics, systems of this generation are the first open-source stream processors capable of computing consistent and accurate results.
-->

---
layout: center
---

#### Real-Time Analytics (also known as "Streaming Analytics")

![Streaming Analytics Architecture](/spaf_0106.png)
<ImageSource work="spaf" />

<!--
Instead of waiting for periodic triggers, a streaming analytics application continuously ingests streams of events and updates its results by incorporating the latest events with low latency. This is similar to the maintenance techniques that database systems use to update materialized views.

_Source: Stream Processing with Apache Flink by Fabian Hueske_
-->

---

### Which Problems Does Flink Solve?

<div class="hypothesis">
Apache Flink solves many issues you might not even realize you have...
</div>

- Event-Time
- Low Latency
- Consistency Guarantees
- Data Locality and Scalability
- A Stream-First Application Programming Model

<!--
Apache Flink is a third-generation distributed stream processor with a competitive feature set. It provides **accurate stream processing with high throughput and low latency at scale**. In particular, the following features make Flink stand out:

**Event-time semantics provide consistent and accurate results despite out-of-order events**. Processing-time semantics can be used for applications with very low latency requirements.

**Exactly-once** means that not only will there be no event loss, but also updates on the internal state will be applied exactly once for each event. This guarantees that our application provides the correct result, as though a failure never happened.

**Millisecond latencies** while processing millions of events per second. Flink applications can be scaled to run on thousands of cores.

**Layered APIs** with varying trade-offs for expressiveness and ease of use.

**Connectors** to commonly used storage systems like Apache Kafka, Apache Cassandra, Elasticsearch, JDBC, Kinesis, and (distributed) filesystems such as HDFS and S3.

Flink allows streaming applications to run **24/7 with minimal downtime** due to its highly available setup (no single point of failure), tight integration with Kubernetes, YARN, and Apache Mesos, quick recovery from failures, and the ability to dynamically scale jobs.

It also allows for the application code of jobs to be updated and jobs to be **migrated** to different Flink clusters **without losing the application's state**.

_Source: Stream Processing with Apache Flink by Fabian Hueske_
-->

---
layout: statement
---

#### Event Time and Watermarks

<style>
    h4 {display: none;}
</style>

Event time is the time when an event in the stream actually occurs, based on a timestamp attached to the events of the stream.

<!--
Timestamps usually exist inside the event data before they enter the processing pipeline (e.g., the event creation time).
-->

---
layout: center
---

![A Watermarked Stream](/spaf_0308.png)
<ImageSource work="spaf" />

<!--
Watermarks are determined (extracted) from records.

**When** they are emitted can be configured or implemented.
Watermarks are special markers in the data stream that indicate the progress of event time.

They help handle out-of-order events in stream processing and allow the system to determine when it can safely process and emit results for a particular window, even if some events are slightly delayed.
-->

---
layout: statement
---

#### Low Latency

Did someone say "Real-time"?

---

My mom says

<div class="hypothesis">"Real-time is when a system is faster than I can react."</div>

<v-clicks>

As software developers, we can say

**Real-time** is processing data as soon as it's emitted.

**Latency** defines how long the consumer has to wait until processing has finished.

**Tolerated Latency** usually depends on the consumer's ability to react.
</v-clicks>

<!--
Disclaimer: My mom didn't actually say that. I derived it from her words when she told me to clean my room "NOW" when I was a child.

As architects, we should encourage analysts to specify tolerated latency prior to designing a system.
Unfortunately, this decision is usually outside of the system boundary but part of the context—which may vary.
-->

---
layout: two-cols
---

#### Data Locality

<img alt="Memory Hierarchy and Read Access Latency" src="/memory-hierarchy.png" class="pd-20 h-50"/>
<ImageSource url="https://cs.brown.edu/courses/csci1310/2020/assign/labs/lab4.html"/>

<template #right>
<v-click>

<h4>Co-Location of Memory and Compute in Flink</h4>

<img alt="In Flink, the data of a task is located at the same node where operations on it are executed." src="/spaf_0104.png" class="pd-20 h-60"/>
  <ImageSource work="spaf" />
</v-click>
</template>

<!--
Having data local at the same node in memory will speed up our **read times by a factor of 1000.**
This is crucial since in **one-by-one processing**, we constantly access data.

"Local" means that the physical memory where the data resides is **on the same compute node** as the CPU processing that data.

Stateless architectures (whether REST or stateless streaming) usually load data from a remote location via network— and even a Redis cache via network takes about 5ms—slow.
-->

---
layout: statement
---

#### Scalability

Alright, let's keep data local. But what do we do if we scale horizontally?

<!--
Scaling horizontally means **adding new nodes** that handle incoming compute demands/traffic.

When maintaining state locally, scaling needs to consider where data is being processed.
-->

---
layout: two-cols
---

##### Keyed Streams and Keyed State

<img alt="Figure 3-12. Tasks with Keyed State" src="/spaf_0312.png" class="m-10 h-80" />
<ImageSource work="spaf" />
<template #right>
<v-click>
<h5>Scaling Stateful (Keyed) Operators</h5>
<img alt="Figure 3-13. Scaling an operator with keyed state in and out." src="/spaf_0313.png" class="m-10 h-80"/>
<ImageSource work="spaf" />
</v-click>
</template>

<!--
Flink allows developers to partition a stream by a logical key.

All operations on a keyed stream will respect data locality: The operator instance of a Flink computational graph will handle all items of the keyed stream that have the same key.
-->

---

#### Consistency Guarantees

- Flink provides "Exactly Once" **processing guarantees** within the job graph.
- **Some sources and sinks** support this out-of-the-box as well.
- Flink's **checkpointing mechanism** ensures these guarantees in case of failures.

<!--
Flink is a distributed data processing system, which must deal with failures such as **killed processes, failing machines, and interrupted network connections.** As tasks maintain their state locally, Flink must ensure that this **state is not lost and remains consistent** in case of a failure.

Flink’s recovery mechanism is based on consistent checkpoints of application state. A consistent checkpoint of a stateful streaming application is a copy of the state of each of its tasks at a point when all tasks have processed exactly the same input.
-->

---
layout: center
---

##### So Tell Me: What is a Checkpoint?!

<style>
    h5 {
        font-size: 2rem;
        margin-bottom: 2rem;
    }

    p {
        font-style: italic;
    }
</style>

A checkpoint is a **consistent** snapshot of the application's state, taken at a regular **interval**.

It serves as a starting point for a failed system to **recover from**.

---
layout: two-cols
---

##### Naive Checkpointing

<style>
    h5 {
        font-weight: 600;
        margin-bottom: 1rem;
    }
</style>

1. <span v-mark="{ color: 'red', type: 'circle' }">Pause</span> the ingestion of all input streams.
2. Wait for all in-flight data to be fully processed, meaning all tasks have processed all their input data.
3. Take a checkpoint by copying the state of each task to a remote, persistent storage. The checkpoint is complete when all tasks finish their copies.
4. Resume the ingestion of all streams.

<template #right>
<v-click>
<h5>Flink's Checkpointing</h5>

1. Inject checkpoint barriers into the stream.
2. The job manager initiates the checkpoint.
3. Each operator externalizes its state.
4. Once all operators acknowledge, the checkpoint is consistent.

</v-click>

<!--
##### Flink’s Checkpointing Algorithm

Flink’s recovery mechanism is based on consistent application checkpoints. The naive approach to taking a checkpoint from a streaming application—to **pause, checkpoint, and resume the application—is not practical for applications** that have even moderate latency requirements due to its “stop-the-world” behavior.

Instead, Flink implements checkpointing based on the Chandy–Lamport algorithm for **distributed snapshots.** This algorithm does not pause the complete application but decouples checkpointing from processing, allowing some tasks to continue processing while others persist their state. The following explains how this algorithm works.

Flink’s checkpointing algorithm uses a special type of record called a checkpoint barrier. Similar to watermarks, checkpoint barriers are injected by source operators into the regular stream of records and cannot be passed by other records. A checkpoint barrier carries a checkpoint ID to identify which checkpoint it belongs to and logically divides the stream into two parts. All state modifications from records that precede a barrier are included in the barrier’s checkpoint, and all modifications from records that follow the barrier are included in a later checkpoint.

The same mechanism can also be used for **savepoints**, which allow for externalizing state at defined points in time (e.g., before an application upgrade).
-->
</template>

---

### Which Problems Does Flink Produce?

<div class="hypothesis">
Apache Flink solves many issues you might not even realize you have...

... but it can also produce some challenges you might not have considered.
</div>

- Complexity: Learning curve
- Monitoring
- State backend
- Job manager/task manager communication
- Running and scaling the infrastructure (Choose your operations model)

<!--
Stream processing requires a **different mental model** compared to state-oriented application programming models.

Additionally, there's an **"event" in "eventual consistency."** While Flink ensures that everything is processed consistently in the end, there may be steps where some parts have been processed while others are still pending. This could require explanations to the user on the UI.

Observing how a continuously running application behaves requires appropriate metrics, which differ from those commonly monitored in RESTful applications.

The interaction between the job manager and task manager, particularly with the state-backend, is not easy to understand. It's deeply integrated into the tooling.
-->

---

### How to Respond to Those Challenges

<v-clicks>

- Acknowledge it's a tricky problem.
- Start small in functional scope, but realistic regarding volume.
- Include other elements of the streaming architecture (Schema Registry!).
- Set up custom metrics from the start.
- Start with a Managed Service (AWS or Azure).

</v-clicks>

<!--
[click]
It's a tricky problem, but **it's a problem Flink is designed to address.**

Still, it's challenging. Allow yourself and your developers time to learn!

Flink provides multiple APIs for different use cases, has excellent documentation, but it's a lot to learn. Java Streaming API, Python, TableAPI, SQL—the more abstract, the more there is to learn.

[click]
A major benefit of Flink is **performance and scalability.** This only works when applied correctly. Pick use cases that challenge your system from the outset.

[click]
As mentioned earlier, Flink can be the central piece of your streaming architecture. However, don't forget the other components and how to integrate them. A **schema registry** can save you from tedious serialization issues downstream if implemented from the outset.

[click]
Flink has an **outstanding metrics system** that exports detailed information about processing state (offset lags, backpressure, parallelism).
The metrics system is easily extensible: Write your own custom metrics to observe your stream's performance.

[click]
Job manager, task manager, and state backend do not need to be understood in detail, as Flink abstracts all this provisioning of compute.
However, if you choose to operate Flink independently, you must dive deep to make it work elastically on your Kubernetes cluster.

There are operators for Kubernetes, Apache Mesos, and bare metal installations, but it's safest to **start with a managed service.** It might not suit your particular use case and may require optimization, but proper configuration needs experience.
-->

---
layout: statement
---

### So (Why) Should I Give Flink a Try?

---
layout: statement
---

“You’re either using a framework, or you’re building your own framework.”

<div>
The React Native team explaining their recommendation to build React Native using Expo.
</div>

---
layout: center
---

![Flink is the hammer for event stream processing](/DALL·E-squirrel-with-hammer.webp)

<!--
**Because Apache Flink is made for this.**

There are challenges to address, but there's always a Flink-native solution. It's a **mature** and **battle-tested** technology.
-->

---
layout: two-cols
---

<img alt="Flink Logo" src="/flink_squirrel_500.png" class="h-90" />
https://flink.apache.org/

<template #right>

<img alt="Stream Processing with Apache Flink by Fabian Hueske and Vasiliki Kalavri" src="/spaf_cover.png" class="h-90" />

</template>
```