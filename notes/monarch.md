## HN discussion
https://news.ycombinator.com/item?id=31379383

The Hacker News discussion on [Monarch: Google’s Planet-Scale In-Memory Time Series Database](https://news.ycombinator.com/item?id=31379383) delves into the intricacies of time series database (TSDB) design, focusing on Monarch's approach to histogram storage and its implications for data analysis and monitoring.([Hacker News][1])

### Key Discussion Points

* **Monarch's Histogram Storage**: Unlike many TSDBs that generate histograms at query time using counter primitives, Monarch stores histograms as a fundamental data type. This design choice aims to improve performance but introduces challenges in flexibility and accuracy during analysis.([Hacker News][1])

* **Rebucketing Limitations**: Users highlight issues with Monarch's inability to rebucket histograms on the fly. This limitation can lead to misleading percentile calculations and artifacts in monitoring graphs, such as sudden jumps or plateaus, especially if the default bucket configurations are not well-suited to specific services.([Hacker News][1])

* **Alternative Solutions**: The discussion references Circonus Histograms, which employ a universal bucketing scheme to address some of these challenges. A co-author of a related paper [on arXiv](https://arxiv.org/abs/2001.06561) contributes insights into this approach.([Hacker News][1])

* **Prometheus Developments**: Participants mention that Prometheus is developing a sparse histogram feature intended to offer dynamic bucketing with minimal overhead. However, there are debates about whether this approach matches the flexibility of Monarch or Circonus.([Hacker News][1])

* **Practical Considerations**: The conversation also touches on the trade-offs between storage efficiency and analytical accuracy. Users discuss strategies for configuring bucket boundaries to align with business objectives and the challenges of maintaining precise monitoring without excessive resource consumption.

Overall, the thread provides a nuanced examination of TSDB design choices, emphasizing the balance between performance optimization and the need for accurate, flexible data analysis in large-scale monitoring systems.([Hacker News][1])

[1]: https://news.ycombinator.com/item?id=31379383&utm_source=chatgpt.com "Monarch: Google’s Planet-Scale In-Memory Time Series Database | Hacker News"


## Peter Kraft on Twitter

How do you keep track of every process running at Google? Countless quadrillions of metrics emitted by billions of devices and VMs?

This is a really cool paper that presents ten years of learnings from running Monarch, a planet-scale in-memory time series database that supports monitoring and alerting for much of Google’s applications and infrastructure. Monarch runs at massive scale, ingesting terabytes of data every second from billions of endpoints in datacenters across the globe, and needs incredible availability as it is a dependency of so many other Google services.

Monarch’s scale and availability requirements informed two critical design decisions: it must prioritize availability over consistency and it must store data in memory. This is because a system like Monarch is most critical when things are breaking–in the case of a network or file system outage, Monarch needs to do the best it can to deliver as much data as possible to the engineers trying to fix it, not get stuck trying to make sure all metrics are consistent and durable. Thus, reads may serve partial data and all writes are best-effort, including replication and flushes to durable storage.

To ingest terabytes of data every second, Monarch adopts a divide-and-conquer strategy. Incoming metrics are routed on multiple dimensions. First, they are routed based on location to a nearby collector. Then, they are lexicographically sharded based on their contents. Once a metric arrives at a Monarch server, it is stored in memory, then asynchronously (with best effort) written to recovery logs. To minimize the amount of data stored, particularly frequent metrics are aggregated during ingestion and only stored as cumulative series, not individual points.

Processing queries in Monarch is also difficult because they have to be globally distributed to get data from all the instances of a service. This is done hierarchically, by multiple levels of query processors that fan out queries across data centers and servers. Each processor level tries to push down work to the lower levels, so as much of the query as possible is evaluated directly on the data. To minimize the degree of fanout, the upper levels store an index of which of their children contain which ranges of data, to ensure queries aren’t sent to children that don’t have relevant data. To ensure availability, if some parts of the system are unresponsive, queries will return partial data.

What are the takeaways? What I found most interesting about Monarch is how it is designed to operate well during a catastrophe. Most databases prioritize strong consistency guarantees–if a query can’t be answered correctly (or data can’t be written durably), it isn’t answered at all. But Monarch needs to work even when everything else is broken, so at every level of its stack it prioritizes availability, getting as much data as possible to the engineers trying to fix a problem. That’s a valuable lesson–match your guarantees to what your users need!

