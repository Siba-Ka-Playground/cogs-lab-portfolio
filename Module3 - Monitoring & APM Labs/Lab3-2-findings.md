# Lab 3.2 Findings: Monitoring Tools Comparison

## What is the difference between what Prometheus/Grafana shows and what SigNoz shows?

While both stacks are used for observability, they focus on different primary pillars:

* **Prometheus & Grafana (Metrics Focus):** This stack is primarily designed for collecting, storing, and visualizing **time-series metrics**. It excels at monitoring the overall health of infrastructure (CPU, memory, disk I/O, network traffic) and broad application-level metrics (request rates, error counts). Prometheus pulls (scrapes) data from targets, and Grafana provides highly customizable dashboards to visualize that data. 
* **SigNoz (Distributed Tracing & APM Focus):** SigNoz is a full-stack Application Performance Monitoring (APM) tool built natively on OpenTelemetry. While it *does* handle metrics and logs, its standout feature is **distributed tracing**. It maps out exactly how a single user request travels through various microservices, databases, and third-party APIs, showing the exact latency and execution time of individual code spans.

## When would you use each during a P1 incident?

During a Priority 1 (P1) total outage or severe degradation, you would typically use these tools in a specific sequence:

**1. Triage with Prometheus & Grafana ("Is the infrastructure on fire?")**
When the P1 pager goes off, Grafana is usually the first screen you look at. 
* **Use it to:** Get a 10,000-foot view of the system. Are servers running out of memory? Is the CPU pinned at 100%? Has network traffic completely dropped off? Is a specific node down?
* **Goal:** Identify *where* the bleeding is happening at the infrastructure or service level (e.g., "The database server's disk is completely full").

**2. Deep Dive with SigNoz ("Why is the application failing?")**
If the infrastructure looks healthy (CPU and RAM are fine), but users are still experiencing errors or massive slowdowns, you switch to SigNoz.
* **Use it to:** Trace specific failing requests. You can look at an active trace and see, for example, that a user login request took 15 seconds because a specific database query inside a microservice is hanging, or a 3rd party payment API is throwing HTTP 500 errors.
* **Goal:** Identify the exact line of code, bad query, or failing microservice causing the application logic to fail.
