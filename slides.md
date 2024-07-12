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
layout: quote
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

<!--well, this is self-explanatory-->

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

Examples: Financial trading, track and trace, traffic based routing (of IP packets and/or goods) -->

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

<!-- there are fundamental differences in those architectures. 

Before looking into streaming architecture components, let's revise a traditional REST appplication -->

---

## Understanding Traditional REST (State) Based Applications

### Definition and Principles

- REST architecture and principles
- How RESTful APIs work in traditional applications

### Request-Response Model

- Explanation of the request-response model
- Limitations in handling real-time data

---

## Introduction to Streaming Data Architecture

### Definition and Principles

- Streaming data architecture overview
- Real-time data processing capabilities

### Event-Driven Architecture

- Principles of event-driven architecture
- Advantages over REST

---

## Key Components of Streaming Data Architecture

### Messaging Systems

- Examples: Kafka, RabbitMQ

### Stream Processing Frameworks

- Examples: Apache Flink, Apache Spark Streaming

### Data Storage for Streaming Data

- NoSQL databases, data lakes

---

## Comparison: Traditional REST vs. *Stateful* Streaming Data Architecture

### Performance

- Latency differences between REST and streaming data architecture
- Overload: Dropped requests vs. Backpressure

### Scalability

- Comparing the scalability of both approaches

### Fault-Tolerance

- Handling failures and ensuring data integrity

---

## Stateless vs. stateful streaming

### What about Spring Cloud, Node-Streams, AWS SNS + Lambda

- All these solutions don't offer built-in-options for state
- State is managed by the DBMS (or Kafka, looking at Kafka-streams)
- State resides on disk
- Simple scaling

### Isn't that what we need?

- Aggregations
- Over time
- Different window types
- Ordering also needs state

---

## Stream processing with Apache Flink

### Usecases

- Event Driven Applications
trigger actions on events, including complex event processing (CEP, which includes identifying patterns in the order of events)
- Realtime analytics
Provide realtime-insights into aggregated events
- Streaming pipelines
Continuously ingest data into warehouses for further (state-oriented) analysis

<!-- https://www.infoworld.com/article/2336241/3-dynamic-use-cases-for-apache-flink-and-stream-processing.html -->

---

## Challenges and Considerations

### Complexity

- Learning curve and operational complexity of streaming architecture

### Data Consistency

- Challenges of maintaining data consistency in streaming environments

### Monitoring and Management

- Importance of monitoring tools and management practices

---


## 11. Q&A

- Open floor for questions and discussion
