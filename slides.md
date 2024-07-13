
---
title: Data in Motion – about being RESTless
transition: fade-out
theme: default
layout: cover
---

# Data in Motion

about being RESTless

---
layout: statement
---

## Why does it matter? And why the hype?

<!--
"Data in Motion" is a term coined (probably) by Confluent

Confluent is a hyped company, major contributor to Apache Kafka, Apache Kafka is hyped in enterprises, so does this alone justify the hype?

No. It's because we try to **represent reality** in software-systems.
-->

---

### Things are now in motion that cannot be undone

<style>
h3 {display: none}
div {background-color: black}
</style>

![ "Things are now in motion that cannot be undone". Gandalf in "Return of the King"](/gandalf-things-in-motion.webp)

<!--
The real world mutates, but all **events that lead to those mutations are bound in time and space** – they are immutable

In software, we achieve constantly great results keeping the **representational gap** low.

You have a Sales Order on paper => Let's model a Sales Order Class and implement a `SlaesOrder` interface – and store it in a `SALES_ORDER_HEADER` and `SALES_ORDER_ITEM` table in the database – or in a `SalesOrders` collection, if you use an object store.

Still, for ages, we mutated our persistence – because everything else would have been **too costly**

Now, that **storage is cheap**, we can finally close this eventual gap – and process those events
-->

---
layout: statement
---

### Using a Stream Processor unlocks the world of ... event processing

... and unlocks new capabilities

<!--
well, this is self-explanatory
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
- **Deutsche Bahn** (currently) uses the Kafka Streams Library to provide insights into trains, schedules and delays in real time to travellers.
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

### Alright, we've got a Kafka up and running, let's build a REST-interface

![https://www.yishizuo.com/dont-be-a-hammer-looking-for-a-nail-3/](/hammer-nail.png)

<!--
there are fundamental differences in those architectures. 

Before looking into streaming architecture components, let's revise a traditional REST appplication
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

<style>
    ul {padding-top: 1rem; list-style-type: none}
    li {padding: 1rem; item-marker: none}
</style>

- Latency
- Scalability
- Statelessness

<!--
- **Latency**: Load of the server slows down the consumer
- **Scalability**: Processing each consumer's requests increases the load of the serer
- **Statelessness**: Each request is independent, making real-time updates complex. We all love optimistic locking, don't we?
-->

---
layout: center
---

## Introduction to Streaming Data Architecture

---

### Characteristics of a Streaming Data Architecture

<style>
    ul {padding-top: 1rem; list-style-type: none}
    li {padding: 1rem; item-marker: none}
</style>

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

<style>
    ul {padding-top: 1rem; list-style-type: none}
    li {padding: 1rem; item-marker: none}
</style>

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

### Key Components of Streaming Data Architecture

<style>
    ul {padding-top: 1rem; list-style-type: none}
    li {padding: 1rem; item-marker: none}
</style>

- **Messaging Systems** like Kafka, RabbitMQ
- **Stream Processing Frameworks** like Apache Flink, Apache Spark Streaming, Azure Streaming Analytics
- **Data Storage for Streaming Data** Like HBase (Hadoop), Cassandra

<!--
Messaging Systems **facilitate the transfer of data** between different parts of the system **in real-time**.

Stream processing frameworks like Apache Flink and Apache Spark Streaming allow for **complex data processing operations on data streams in real-time**

NoSQL databases and data lakes **store vast amounts of real-time data efficiently**.
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
    Horizontally on the appserver, <br/> vertically on the DBMS
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
<style>
.statement {font-size: 2rem; line-height: unset; font-style: italic}
</style>

<div class="statement">
Ok, you got me. we need event stream processing.
</div>
<div class="statement">
We already got Kafka, we're fine... No?
</div>
---
layout: two-cols
---

## Stateless vs. stateful streaming

<style>
    h2 {display: none}
</style>

### What about Spring Cloud, Node-Streams, AWS SNS + Lambda

**Stateless** processing

- one-by-one-processing
- Filtering
- Mapping (data enrichment)

<template #right>
<h3>So what do we need state for?</h3>

**Stateful** computations

- Aggregations (over time)
- Business processing which rely on ordering
- complex event processing
</template>
<!--

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

## Stream processing with Apache Flink

<style>
    h2 {margin: auto}
</style>

---
layout: center
---

### Integration of Flink into the streaming landscape


![The Flink-Kafka-ecosystem](/public/flink-kafka-ecosystem.png)

---


### Usecases

<style>
    ul {padding-top: 1rem; list-style-type: none}
    li {padding: 1rem; item-marker: none}
</style>

- Event Driven Applications
- Realtime analytics
- Streaming pipelines

<!-- https://www.infoworld.com/article/2336241/3-dynamic-use-cases-for-apache-flink-and-stream-processing.html -->

<!--
- Event Driven Applications
  trigger actions on events, including complex event processing (CEP, which includes identifying patterns in the order of events)
- Realtime analytics
  Provide realtime-insights into aggregated events
- Streaming pipelines
  Continuously ingest data into warehouses for further (state-oriented) analysis
-->

---

### Which problems does Flink solve

- Data locality
- Scalability
- Delivery guarantees

---

#### Data locality

<img alt="Memory Hierarchy and read access latency" src="/memory-hierarchy.png" class="m-10 h-90"/>
<div class="source">https://cs.brown.edu/courses/csci1310/2020/assign/labs/lab4.html?spm=a2c65.11461447.0.0.47497f65FTyCWF</div>

<!--
Having data local at the same node in-memory will speed up our **read times by factor 1000**
In one-by-one-processing, we constantly access data
-->

---

#### Scalability

Alright, let's keep data local. But what do we do if we scale horizontally?

---

#### Delivery guarantees

What happens if the stream breaks? How do we recover

=> Timestamps, event time and watermarks


---

### Which problems does Flink produce

- State backend
- Job manager / task-manager communication
- Serialization
- Monitoring


---

## Challenges and Considerations

---

### Complexity

- Learning curve and operational complexity of streaming architecture
- Mental model

<!--
Acknowledge the complexity involved in setting up and maintaining a streaming data architecture. Mention the steep learning curve and the operational challenges that can arise, such as managing data streams, ensuring fault tolerance, and scaling the system.
-->

---

### Data Consistency

There's an "event" in eventual consistency

<!--
Discuss the challenges in maintaining data consistency in a streaming environment. Explain that because data is processed in real-time and often across distributed systems, ensuring consistency can be more complex compared to traditional architectures.
-->

---

### Monitoring and Management

- Importance of monitoring tools and management practices
- Need for appropriate metrics

<!--
Emphasize the importance of robust monitoring and management practices in streaming data architectures. Highlight the need for tools that can provide real-time visibility into data streams, detect anomalies, and manage system health to ensure smooth operation.
-->

---

