## 🚀 High-Throughput, Low-Latency Data Transport

* Scribe serves as **Meta’s primary data transport layer**, ingesting over **15 TB/s** of data and delivering more than **110 TB/s** to consumers.
* It guarantees **write availability** for hundreds of millions of heterogeneous producers, treating inputs as Meta’s **“golden source of truth.”**

---

## 🏗️ Architectural Innovations

1. **Multi-hop Write Path**

   * Uses **buffering and non-deterministic routing/placement** of data to maximize write availability.
   * Write proxies and batch services dynamically route, compress, and persist messages in scalable ways.

2. **Decoupled Storage Model**

   * Metadata is stored in **LogDevice**, while payloads go to **Tectonic**, Meta’s distributed filesystem.
   * This separation enables durability, efficient batch storage, and flexibility in retrofitting stronger delivery guarantees.

3. **Ephemeral Storage Layer**

   * Combines **Memcache** (1–2 hour retention) and **CacheLib in Read Proxies** for hot data caching.
   * Reduces cross-region traffic, durable storage I/O, and enables **columnar views** for analytics.

4. **Disaggregated Read Path**

   * Separates **metadata streaming** (Read Stream Service) from **payload serving** (Read Proxy).
   * Improves load balancing, fault isolation, and supports **query pushdown and materialized views**.

---

## 📊 Flexibility & Multi-Tenancy

* Supports **independent data streams (“categories”)** with configurable retention, partitioning (sharding), and ordering semantics.
* Enables **multi-tenant resource sharing** with traffic shaping, quotas, and affinity-based load balancing to protect against noisy neighbors.
* Consumers can filter with **SQL expressions**, request custom serialization formats, or read only subsets of a dataset.

---

## 🔒 Delivery & Ordering Guarantees

Scribe exposes multiple **levels of guarantees**, allowing applications to trade off cost, scalability, and correctness:

1. **Best-effort** – fire-and-forget writes, approximate ordering.
2. **At-least-once** – storage acknowledgments + retries to ensure persistence.
3. **Repeatable reads** – deterministic ordering for categories.
4. **Exactly-once processing** – via deduplication and sequencing mechanisms.

---

## 🧩 Lightweight ETL & Data Staging

* Minimizes downstream overhead by:

  * **Transcoding row-oriented writes** (Thrift) into **columnar formats** (PrestoPage/Velox).
  * Supporting **filter pushdowns** and **materialized views**, reducing data volumes and deserialization costs.

---

## 📚 Lessons Learned (18 Years of Evolution)

* **Shift from binaries to libraries:** started with a simple binary API, moved to **typed Producer/Consumer libraries** for better safety and extensibility.
* **Best practices enforcement:** evolved from “encouragement” to **contract-based regionalized capacity planning** for predictability.
* **Vertical integration with downstream systems:** e.g., co-partitioning with warehouse tables, adopting ML-specific columnar encodings.
* **Incremental modernization:** from opaque payload transport to a **data staging service** supporting analytics, privacy enforcement, and stronger guarantees.

---

## ✅ In Summary

The **key contributions of Scribe** are:

1. **Scalable design** supporting >100 TB/s egress across Meta’s infrastructure.
2. **Multi-hop, decoupled architecture** for durability, efficiency, and fault tolerance.
3. **Flexible consumption model** with filtering, sharding, and multiple delivery guarantees.
4. **Ephemeral caching & lightweight ETL** to reduce downstream costs.
5. **Operational lessons** from nearly two decades of running critical infrastructure at hyperscale.

# Comparison with Kafka


## 🌍 **Deployment Context**

* **Scribe**

  * Built and operated *internally at Meta* for 18+ years.
  * Tailored for hyperscale: **15 TB/s ingress, 110+ TB/s egress**.
  * Designed around Meta’s massive multi-tenant workload, global data centers, and deep integration with Meta’s compute/storage stack.

* **Kafka**

  * Open-source, widely adopted industry standard.
  * Runs well in enterprise-scale clusters, but typically below Meta’s hyperscale throughput.
  * Ecosystem-driven with Confluent & others providing managed/cloud versions.

---

## 📦 **Data Model & APIs**

* **Scribe**

  * Uses **categories** (logical streams), sharded across millions of producers.
  * Payloads are **schema-driven (Thrift + Tulip registry)** rather than arbitrary blobs.
  * Consumers can filter with **SQL-like expressions**, and request **columnar (PrestoPage) output** for analytics.

* **Kafka**

  * Uses **topics** with **partitions**.
  * Payloads are opaque byte arrays (schema enforcement is optional via Schema Registry, not core).
  * Consumers get raw messages; filtering/transcoding must be done downstream.

---

## ⚙️ **Architecture**

* **Scribe**

  * **Multi-hop write path**: Producer → ScribeD → Write Proxy → Batch Service → LogDevice metadata + Tectonic payload store.
  * **Decoupled storage**: metadata in **LogDevice**, payloads in **Tectonic FS**.
  * **Ephemeral caches** (Memcache, CacheLib) for hot data and **materialized views** to reduce deserialization costs.
  * Heavy **integration with Meta infra** (Velox, Presto, Flux schedulers, privacy enforcement).

* **Kafka**

  * **Single-hop write path**: Producer → Broker → storage.
  * Metadata and payloads stored together on brokers.
  * No built-in ephemeral caches or materialized views; relies on external tools (Kafka Streams, ksqlDB, Flink).
  * Simpler architecture, but fewer native optimizations for analytics.

---

## 🔄 **Scalability & Performance**

* **Scribe**

  * Optimized for **heterogeneous workloads**: some categories get **500 GB/s firehoses**, others just a trickle.
  * Handles **hundreds of consumers per stream** across global data centers.
  * Read path disaggregation (Read Stream Service + Read Proxy) prevents hotspots, supports high fan-out.

* **Kafka**

  * Scales well, but throughput per partition is limited by broker I/O.
  * Large fan-out possible, but cross-region replication requires **MirrorMaker** or Confluent Replicator (extra complexity).
  * Heavy consumers can still cause **broker hotspots**.

---

## 📜 **Delivery & Ordering Guarantees**

* **Scribe**

  * Offers a **menu of guarantees**:

    * Best-effort (fire-and-forget, approximate order).
    * At-least-once.
    * Repeatable reads.
    * Exactly-once processing (via deduplication, sequencing).
  * Consumers can **configure ordering windows** (e.g., tolerate minutes of disorder for lower latency).

* **Kafka**

  * Provides **at-least-once** semantics by default.
  * **Exactly-once** is possible (idempotent producers + transactional APIs).
  * Ordering is guaranteed **per partition only** (not across partitions).

---

## 👥 **Multi-Tenancy & Resource Management**

* **Scribe**

  * Built-in **traffic shaping, quota management, and load-aware routing** at every level (producer, proxy, batcher, read proxy).
  * Per-tenant **SLO monitoring**, error attribution, and capacity contracts.

* **Kafka**

  * Multi-tenancy possible but less sophisticated.
  * Relies on quota configs per client ID/topic, but not deeply integrated.
  * Monitoring usually external (Prometheus, Grafana, Confluent Control Center).

---

## 📊 **Lightweight ETL & Analytics**

* **Scribe**

  * Transcodes row-oriented payloads into **columnar formats** (PrestoPage/Velox).
  * Performs **predicate pushdown** and **materialized views** at the read proxy.
  * Effectively acts as a **data staging + transport system**.

* **Kafka**

  * Core Kafka does no transformation.
  * ETL/analytics must be layered on top (Kafka Streams, Flink, Presto).
  * This separation makes Kafka more flexible for general-purpose event streaming, but less optimized for large-scale analytics.

---

## 📝 **In Summary**

* **Scribe** is like **Kafka on hyperscale steroids**:

  * Deeply integrated with Meta infra.
  * Prioritizes **write availability** above all.
  * Adds **caching, lightweight ETL, and flexible guarantees** to reduce downstream cost.
  * Tailored to **Meta’s scale and diversity** of workloads.

* **Kafka** is:

  * General-purpose, simpler to deploy, and **open-source**.
  * Strong ecosystem (Kafka Streams, Connect, ksqlDB).
  * Well-suited for most enterprises, but **less specialized for analytics-heavy, cross-region, 100+ TB/s use cases**.

## Side-by-side comparison

# 📊 Scribe vs. Kafka: Key Differences

| Aspect                  | **Scribe (Meta)**                                                                                                                                                               | **Apache Kafka**                                                                                                      |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Origin & Scope**      | Internal at Meta, evolved over 18+ years, tailored for hyperscale workloads (>15 TB/s ingress, >110 TB/s egress)                                                                | Open-source, industry standard, widely adopted across enterprises                                                     |
| **Data Model**          | **Categories** (logical streams), schema-driven (Thrift + Tulip registry), optional SQL filters & columnar output (PrestoPage/Velox)                                            | **Topics** with partitions, opaque byte arrays (schema optional via external registry)                                |
| **Architecture**        | Multi-hop write path: Producer → ScribeD → Write Proxy → Batch Service → LogDevice (metadata) + Tectonic (payloads); disaggregated read path (Read Stream Service + Read Proxy) | Simpler broker-based: Producer → Broker → storage; metadata and payload stored together                               |
| **Storage**             | Decoupled: metadata in **LogDevice**, payloads in **Tectonic FS**; supports **ephemeral caching (Memcache + CacheLib)** and **materialized views**                              | Brokers persist both metadata and payloads on local disks; no built-in ephemeral caches or materialized views         |
| **Performance & Scale** | Optimized for heterogeneous traffic: some categories hit **500 GB/s**; supports **hundreds of consumers per stream** globally                                                   | High throughput but limited per-partition; scaling across regions requires MirrorMaker/Replicator                     |
| **Guarantees**          | Flexible: best-effort, at-least-once, repeatable reads, exactly-once; supports **ordering windows** for configurable trade-offs                                                 | At-least-once default; exactly-once with idempotent producers + transactions; ordering guaranteed only per partition  |
| **Multi-tenancy**       | Built-in **traffic shaping, quotas, load-aware routing, SLO monitoring**, per-tenant error attribution                                                                          | Supports quotas per client/topic, but multi-tenancy features are less advanced and mostly externalized                |
| **ETL & Analytics**     | Integrated **lightweight ETL**: columnar transcoding, predicate pushdown, materialized views; reduces downstream deserialization cost                                           | ETL/analytics handled by external tools (Kafka Streams, Flink, ksqlDB, Presto); Kafka core only stores/forwards       |
| **Use Case Fit**        | Best for **hyperscale, analytics-heavy, cross-region, multi-tenant workloads**; tightly coupled with Meta infra (Velox, Presto, Flux schedulers, privacy systems)               | Best for **general-purpose event streaming** across industries; easier adoption, flexible ecosystem, strong community |


