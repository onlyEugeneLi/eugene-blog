# System Design

# Scalability

A service is said to be **scalable** if, as **resources** are added to the system, it results in **increased performance** in a manner proportional to resources added.

An always-on service is said to be scalable if **adding resources** to facilitate **redundancy** does **not** result in a **loss of performance**.

## Horizontal Scaling

**cloning** of an application or **service** such that work can easily be distributed across instances with absolutely no bias.

and implement a **load balancer**

# Patterns

## Scalability Pattern: Load Balancing
---
### Common Load Balancing Methods

**Least Connection Method** \
This method directs traffic to the server with the fewest active connections. Most useful when there are a large number of persistent connections in the traffic unevenly distributed between the servers. Works if clients maintain long-lived connections.

**Least Response Time Method** \
This method directs traffic to the server with the fewest active connections and the lowest average response time. Here, response time is used to provide feedback of the server’s health.

**Round Robin Method** \
This method rotates servers by directing traffic to the first available server and then moves that server to the bottom of the queue. Most useful when servers are of equal specification and there are not many persistent connections.

**IP Hash** \
The IP address of the client determines which server receives the request. This can sometimes cause skewness in distribution but is useful if apps **store some state locally** and need some **stickiness**.

More advanced client/server-side example techniques:

- https://docs.nginx.com/nginx/admin-guide/load-balancer/
- https://cbonte.github.io/haproxy-dconv/2.2/intro.html#3.3.5
- https://twitter.github.io/finagle/guide/Clients.html#load-balancing

> Make copies of components


## Scalability Pattern: Caching—Content Delivery Networks (CDN)

CDNs are added closer to the client’s location. 

> Cache static data or lambda function closer to client's location

## Scalability Pattern: Microservices

the separation of work by service or function within the application

## Scalability Pattern: Sharding

This pattern represents the <span style='color:red'> separation</span> of work based on attributes that are looked up to or determined at the time of the transaction. Most often, these are implemented as <span style='color:red'>splits</span> by requestor, customer, or client.


## High availability 

**Availability Parallel Components**
If we have more than one LB and if the rest of the LBs can take over the traffic during one LB failure, then LBs are operating in parallel.

#### 📊 Availability "Nines" vs Downtime
---

| Availability | Max Downtime per Year |
|--------------|------------------------|
| 90.0%        | 36.5 days              |
| 95.0%        | 18.25 days             |
| 99.0%        | 3.65 days              |
| 99.5%        | 1.83 days              |
| 99.9% (3 nines)  | 8.76 hours              |
| 99.99% (4 nines) | 52.6 mins              |
| 99.999% (5 nines)| 5.26 mins              |
| 99.9999% (6 nines)| 31.5 secs              |


### Core Principles

**Elimination of <span style='color:red'>single points of failure</span> (SPOF)** This means adding **redundancy** to the system so that the failure of a component does not mean failure of the entire system.

---

**Reliable crossover** In redundant systems, the crossover point itself tends to become a single point of failure. Reliable systems must provide for reliable crossover.

- a crossover refers to a mechanism that enables a smooth transition of operations from a failing component to a redundant, functioning component, ensuring minimal or no downtime.

> Crossover -- switching over to the backup \
> Make sure the plan B works

---
**Detection of failures as they occur** If the two principles above are observed, then a user may never see a failure.

## Fault Tolerance (Reliability)

### Fault Tolerance: Failure Metrics

**Mean time to repair (MTTR)**: The average time to repair and restore a failed system.

**Mean time between failures (MTBF)**: The average operational time between one device failure or system breakdown and the next.

**Mean time to failure (MTTF)**: The average time a device or system is expected to function before it fails.

**Mean time to detect (MTTD)**: The average time between the onset of a problem and when the organization detects it.

**Mean time to investigate (MTTI)**: The average time between the detection of an incident and when the organization begins to investigate its cause and solution.

**Mean time to restore service (MTRS)**: The average elapsed time from the detection of an incident until the affected system or component is again available to users.

**Mean time between system incidents (MTBSI)**: The average elapsed time between the detection of two consecutive incidents. MTBSI can be calculated by adding MTBF and MTRS (MTBSI = MTBF + MTRS).

**Failure rate**: Another reliability metric, which measures the frequency with which a component or system fails. It is expressed as a number of failures over a unit of time.


### Fault Isolation Terms

If “Notifications” is not working, the site should gracefully handle that failure by removing the functionality instead of taking the whole site down.

#### Swinlane

Fault isolation methodologies

**Swimlane Principles**

- Principle 1: *Nothing is shared* (also known as “share as little as possible”). The less that is shared within a swimlane, the more fault isolative the swimlane becomes. (as shown in Enterprise use-case)

- Principle 2: *Nothing crosses* a swimlane boundary. Synchronous (defined by expecting a request—not the transfer protocol) communication never crosses a swimlane boundary; if it does, the boundary is drawn incorrectly. (as shown in Ads feature)

Swimlane adds a barrier to the service from other services so that failure on either of them won’t affect the other.

if we swimlane the “Generation of Ads” service and use a shared storage to populate Newsfeed App, Ads failures won’t cascade to Newsfeed, and worst case if Ads don’t meet SLA, we can have Newsfeed without Ads.

## Chaos Engineering

Chaos engineering encompasses techniques aimed at meeting resilience requirements.

Chaos engineering can be used to achieve resilience against infrastructure failures, network failures, and application failures.

An evaluation to induce chaos in a Kubernetes environment terminated random pods receiving data from edge devices in data centers while processing analytics on a big data network. The pods' recovery time was a resiliency metric that estimated the response time.

## Numbers every programmer should know

[Numbers every programmer should know](https://colin-scott.github.io/personal_website/research/interactive_latency.html)

# Large System Design

## SLIs and SLOs

- **service level indicator(SLI)**: a carefully defined quantitative measure of some aspect of the level of service that is provided.
- **Service level objective (SLO)**: a **target** value or range of values for a service level that is measured by an **SLI**. 

### Core components of SRE



### Golden signals of SRE

- Latency
- Traffic
- Errors
- Saturation

# Scaling

## Stateful vs Stateless services

A **stateless** process or service doesn’t rely on stored data of it’s past invocations. 

A **stateful** service on the other hand stores its state in a datastore, and typically uses the state on every call or transaction.

---

> An user with a million followers can potentially lead to hundreds of thousands of calls to the DB, simply to fetch the latest photoID that the user has uploaded. This can quickly overwhelm any DB, and can potentially bring down the DB itself.


## Sharding

One way to solve the problem of DB limitation is scaling up the DB write and reads.

Sharding is one way to scale the DB writes, where the **DB** would be **split** into parts that reside in different instances of the DB running on **separate machines**. 

DB reads can be scaled up similarly by using **read replicas** as we had seen in Phase 1 of this module.

## Caching

# Resiliency

A resilient system is one that can keep functioning in the face of adversity.

Resilient architectures leverage **system design patterns** such as **graceful degradation**, **quotas**, **timeouts** and **circuit breakers**. 

## Quotas - Rate limiters

assign a specific quota for each component - by way of specifying requests per unit time

## Graceful Degradation
When a system with multiple dependencies encounters failure in one of the dependencies, gracefully **degrading** to **minimum viable functionality** would be a lot better than grinding the entire system to a halt.

## Timeouts

When calling such a resource (database, API endpoints) from our application, it is important to always have a reasonable timeout.

A reasonable time out is helpful to keep the user experience consistent - it is better to fail rather than to have frustratingly long delays, in some cases.


## Exponential back-offs

At large enough scale, the retries can compete with the new requests (which might very well be served as expected) and saturate the system. To avoid this, we can look at exponential back-off for retries. This essentially decreases the rate at which the clients retry, upon encountering consecutive failures on retries.



## Circuit breakers

## Self healing systems

A self-healing system adds more instances in this scenario to replace the failed instances. <span style='color:red'>**Auto-scaling**</span> like this can also help when there is a sudden spike in query. If our application runs on a public cloud, it might simply be a matter of spinning up more virtual machines. 


# Backups

## Types of backups

[Types of Backups](https://www.spanning.com/blog/types-of-backup-understanding-full-differential-incremental-backup/)


full, differential, and incremental