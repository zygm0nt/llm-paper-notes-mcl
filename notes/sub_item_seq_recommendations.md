TLDR
This work introduces **PQTopK**, a parallelisable scoring algorithm for compressed Transformer-based recommenders. It preserves recommendation quality, dramatically reduces inference time (especially at million+ item scale), and enables practical deployment of such models in industry.

Related article [Restructuring Vector Quantization with the Rotation Trick](rotation_trick.md)

## Summary

### **Problem and Motivation**

* Transformer-based recommender systems (e.g., SASRec, BERT4Rec) achieve state-of-the-art results in sequential recommendation tasks.
* In real-world production, these models struggle with **large catalogues** (millions of items), due to:

  * High memory usage for item embeddings.
  * Slow inference times.
  * Difficulty deploying on CPU-only hardware, which is common in practice due to GPU costs .

---

### **RecJPQ and Its Limitations**

* **RecJPQ** (2024) compresses item embeddings by decomposing them into **shared sub-item IDs** using Product Quantisation (PQ).
* This reduces memory use by up to **50×** while preserving effectiveness.
* However, RecJPQ’s **scoring algorithm** is inefficient:

  * It requires iterative accumulation of scores for each item.
  * This prevents effective parallelisation, making inference slow .

---

### **PQTopK Algorithm (Proposed Improvement)**

* Inspired by **LightRec**, the authors introduce **PQTopK**:

  * Pre-computes scores for sub-item IDs.
  * Allows fast recombination of these scores for items.
  * Enables efficient **parallelisation**.
* Reduces per-item scoring cost from $d$ multiplications to only $m$ additions ($m \ll d$) .
* Can handle catalogues with **hundreds of millions of items** efficiently.

---

### **Experimental Setup**

* Datasets: **Booking.com** (\~35K items) and **Gowalla** (\~1.3M items).
* Backbone models: **SASRec** and **gBERT4Rec** (variant of BERT4Rec with negative sampling).
* Evaluation metrics:

  * **Median response time per user (mRT)** for inference efficiency.
  * **NDCG\@10** for effectiveness (unchanged across methods).
* Hardware: CPU-only, no GPU acceleration .

---

### **Results**

* **On small datasets (Booking.com):**

  * Backbone inference dominates runtime; differences between scoring methods are minor.
* **On large datasets (Gowalla, >1M items):**

  * PQTopK achieves **4.5× speedup over default Transformer scoring** and **1.56× over RecJPQ scoring** with SASRec.
  * Scoring time with PQTopK becomes negligible compared to the Transformer backbone itself .
* **Scalability tests (simulated up to 1B items):**

  * PQTopK remains efficient up to **100M items**.
  * At **1B items**, inference takes \~10 seconds per user (not suitable for interactive use but usable for pre-computed recommendations).
  * Default matrix multiplication fails beyond \~10M items due to memory limits .

---

### **Conclusions**

* The paper demonstrates the **first analysis** of inference in sub-ID-based sequential models at large scale.
* **PQTopK enables efficient CPU-only inference** with catalogues of millions of items, overcoming a major obstacle for production deployment.
* Findings suggest this approach makes Transformer-based recommenders feasible in **real-world, large-scale systems** .

Would you like me to also create a **one-page executive-style summary** (with bullet points and a visual workflow from the diagrams on pages 2–3) for quick sharing with colleagues?
