## Photon: A Fast Query Engine for Lakehouse Systems

From: https://x.com/gwenshap/status/1924207478360977751

> Photon is Databricks' C++ engine hiding under Spark SQL — and the SIGMOD '22 paper is full of 🔥 for engine nerds. They explain:
> 
> - Why they ditched JVM for native code
> - Vectorized interpreted execution (not codegen!)
> - Runtime tricks like SIMD ASCII checks and UUID rewrites
> - Benchmarks: TPC-H 4× faster, Q1 23× faster
> 
> Worth reading for the design decisions and a bunch of useful optimization techniques. 
> 
> Section 3 does a great job explaining design considerations and real-world tradeoffs. Sections 4–5 offer a collection of engine-level optimizations you’ll want to borrow.


## Summary

The paper **"Photon: A Fast Query Engine for Lakehouse Systems"** presents *Photon*, a native, vectorized, columnar query engine developed by Databricks to improve performance in Lakehouse architectures. Here are the key learnings:

---

### 🔧 **Motivation and Context**

* **Lakehouse Architecture** merges the capabilities of data warehouses (e.g., governance, ACID transactions) with the scalability and flexibility of data lakes.
* Traditional engines struggle with raw, uncurated data common in data lakes and often require ETL processes into separate warehouses.
* Photon is built to eliminate this complexity and provide **fast query performance** directly on Lakehouse data.

---

### 🧠 **Core Design Principles**

1. **Native Engine in C++**

   * Avoids JVM overhead and allows fine-grained control over memory and CPU.
   * Enables optimizations like SIMD, memory pipelining, and low-level tuning not feasible in JVM.

2. **Vectorized, Columnar Execution**

   * Inspired by MonetDB/X100, Photon processes data in column batches to enhance CPU utilization.
   * Leverages SIMD and adaptive optimizations for NULLs, data sparsity, and ASCII encoding.

3. **Interpreted vs. Code-Generated Execution**

   * Chose **interpreted vectorized execution** over runtime code generation (as in Spark SQL or HyPer).
   * Easier to debug, extend, and monitor.
   * Still allows specialized fused operators for common expressions (e.g., `between`, `upper()`).

4. **Adaptive Execution**

   * Dynamically optimizes based on runtime data characteristics (e.g., sparse batches, ASCII strings).
   * Adapts kernel choice at runtime based on batch metadata.

---

### 🔄 **Integration with Spark**

* Photon is embedded within the **Databricks Runtime (DBR)** and works seamlessly with existing Spark APIs.
* Uses **Catalyst rules** to selectively convert Spark SQL plans into Photon-compatible plans.
* Supports **partial rollout**—Photon executes parts of a query while falling back to Spark SQL as needed.

---

### 🧩 **Engineering Features**

* **Columnar memory model** with position lists to handle active rows and maintain SIMD efficiency.
* **Efficient memory management**:

  * Transient allocations use buffer pools.
  * Adaptive spilling coordinated with Spark’s unified memory manager.
* **Hash tables** and **aggregations** optimized for vector access.

---

### 📊 **Performance Results**

* **3×–23× speedups** on real customer queries and benchmarks.
* **TPC-H SF=3000**: Average 4× speedup over Databricks Runtime (max 23× on Q1).
* **Photon enabled Databricks to set the world record** for 100TB TPC-DS benchmark as of 2022.

---

### 🧪 **Testing and Compatibility**

* Extensive testing ensures **semantic consistency** with Spark:

  * Unit tests, end-to-end comparisons, and fuzz tests.
* Photon handles subtle issues like **timezone handling** and **decimal arithmetic differences** across JVM and native code.

---

### 📌 Key Takeaways

* **Photon succeeds by blending columnar, vectorized, and adaptive techniques** within a native engine, while maintaining compatibility with Spark workloads.
* It provides **enterprise-grade performance** on open data formats (like Delta Lake) without the need to move data into specialized warehouses.
