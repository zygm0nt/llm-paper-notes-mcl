## Summary

The article introduces the "Yi" model family, comprising 6B and 34B parameter language and multimodal models, designed for high performance across benchmarks like MMLU and human-preference evaluations. These models are built on a data-cleaning pipeline that prioritizes quality over quantity, utilizing 3.1 trillion tokens for pretraining and focusing on efficiency for deployment on consumer-grade devices through advanced quantization techniques. Extensions to the models include long-context capabilities, vision-language integration, and iterative fine-tuning processes, showcasing competitive performance near GPT-3.5 and emphasizing scalable, high-quality development for AI applications.

## Model architecture

The **Yi model architecture** is based on a modified version of the classical **decoder-only Transformer architecture**, inspired by implementations such as LLaMA. It incorporates several design and efficiency features:

1. **Core Architecture**:
   - **Transformer-Based Design**: Standard dense Transformer architecture optimized for high performance across tasks.
   - **Grouped-Query Attention (GQA)**: Used in both Yi-6B and Yi-34B models, reducing training and inference costs without compromising performance. GQA groups query heads to share key and value heads within each group, optimizing efficiency compared to Multi-Head Attention (MHA).
   - **SwiGLU Activation**: Replaces standard activation functions to reduce parameter size while maintaining computational efficiency and alignment with model design.

2. **Positional Embedding**:
   - **Rotary Position Embedding (RoPE)**: With an adjusted base frequency (RoPE ABF) to support context lengths up to 200K tokens. This adjustment enhances the model's capacity to handle extended contexts without architectural changes like sparse or sliding-window attention.

3. **Configuration Details**:
   - **Yi-6B**: 4096 hidden size, 32 attention heads, 32 layers, pretrained on sequences up to 4096 tokens.
   - **Yi-34B**: 7168 hidden size, 56 attention heads, 60 layers, also pretrained with 4096 tokens.
   - Both models can be quantized to 8-bit or 4-bit formats for efficient deployment on consumer-grade hardware.

4. **Optimization for Long Contexts**:
   - The architecture inherently supports long-context learning by leveraging lightweight continual pretraining and finetuning with sequences up to 200K tokens, enabling strong retrieval performance.

5. **Adaptability**:
   - The architecture is designed to integrate additional capabilities, such as vision-language tasks (via a vision transformer encoder) and depth upscaling to create larger models with improved performance.

These architectural choices reflect a balance between performance, scalability, and deployment efficiency, enabling the Yi models to achieve competitive results while being cost-effective for real-world applications.

## Innovation

The Yi model family introduces several **innovative approaches** across model design, training, and deployment that distinguish it from traditional methodologies:

### 1. **Data Engineering Innovation**
   - **High-Quality Pretraining Data**: A meticulously engineered pipeline processes 3.1 trillion tokens of English and Chinese data, using cascaded filtering techniques for deduplication, quality, and safety. This approach prioritizes data quality over quantity, deviating from many models that rely on sheer data volume.
   - **Small, Curated Fine-Tuning Dataset**: Less than 10K instruction-response pairs are handcrafted and iteratively refined with user feedback, promoting higher accuracy and relevance compared to massive-scale but less-polished datasets used by competitors.

### 2. **Efficiency in Model Scaling**
   - **Model-Data Trade-Off**: Yi's smaller 34B model achieves performance parity with larger models (e.g., GPT-3.5) by compensating for fewer parameters with more training data and superior quality. This is consistent with the "post-Chinchilla optimal regime," optimizing inference cost while maintaining strong performance.
   - **4-bit and 8-bit Quantization**: Yi models can run efficiently on consumer-grade GPUs, such as RTX 4090, with minimal performance degradation after quantization, broadening accessibility for developers and researchers.

### 3. **Long-Context Processing**
   - **200K Context Lengths**: Unlike many models that require architectural changes (e.g., sparse attention) for extended contexts, Yi retains full attention mechanisms and achieves long-context capabilities through lightweight continual pretraining and finetuning. This is complemented by exceptional performance in retrieval tasks like the Needle-in-a-Haystack test.

### 4. **Innovative Model Extensions**
   - **Vision-Language Integration**: By combining a language model with a vision transformer encoder, Yi adapts seamlessly to multimodal tasks, aligning visual representations with the language model's semantic space.
   - **Depth Upscaling**: Yi leverages a unique approach to extend model depth (e.g., from 6B to 9B parameters) by duplicating specific layers without requiring additional pretraining, preserving computational efficiency while improving performance.

### 5. **Infrastructure Advancements**
   - **Full-Stack Support**: Automated task scheduling, failure recovery, and topology-aware resource allocation optimize training and finetuning across distributed systems, reducing overhead and ensuring scalability.
   - **Dynamic Batching and Paged Attention**: These techniques enhance memory usage and decoding speed, critical for efficient large-scale deployment.

### 6. **Bilingual Focus**
   - **English and Chinese Expertise**: Yi’s bilingual training ensures competitive performance on tasks involving both languages. For example, it surpasses GPT-4 on some Chinese knowledge-related benchmarks (e.g., C-Eval, CMMLU).

### 7. **Alignment and Safety**
   - **Iterative Human Feedback**: Direct input from machine learning engineers ensures alignment with user needs, unlike models relying solely on automated data generation.
   - **Comprehensive Safety Measures**: A Responsible AI Safety Engine (RAISE) enforces ethical guidelines during pretraining, alignment, and deployment, enhancing trustworthiness and reducing risks.

### Why It’s Innovative:
The Yi approach challenges conventional norms by:
   - Shifting focus to **data quality** and **engineering precision** over raw scale.
   - Optimizing for **real-world usability** with consumer-grade hardware compatibility.
   - Introducing **scalable techniques** like depth upscaling and long-context modeling without major architectural modifications.
   - Balancing **multilingual capability**, multimodality, and cost-effective deployment.

This combination of strategies enables the Yi models to deliver cutting-edge performance while being efficient, accessible, and aligned with user and ethical standards.


## Data pipeline

The **data preparation** approach for the Yi model family is highly innovative and rigorous, focusing on maximizing quality over quantity. Here are the key highlights:

---

### 1. **Sophisticated Data Pipeline**
   - **Data Scale**: 3.1 trillion tokens, bilingual in English and Chinese, making it one of the largest high-quality corpora used for pretraining.
   - **Cascaded Filtering Pipeline**:
     - **Language Identification**: Starts with Common Crawl and uses the CCNet pipeline for language detection and perplexity scoring.
     - **Heuristic Filters**: Removes low-quality text based on:
       - Domain, URL blocklists, and document length.
       - High ratios of special symbols, incomplete lines, repeated patterns, or garbled text.
       - Detection and anonymization of personally identifiable information (PII).
     - **Learned Filters**:
       - **Perplexity Scorer**: Evaluates coherence by analyzing perplexity scores and discards poorly structured data.
       - **Quality Scorer**: Prefers Wikipedia-like content.
       - **Safety Scorer**: Identifies and removes harmful or unsafe content like pornography, violence, or propaganda.
       - **Document Coherence Scorer**: Filters documents with unrelated or incoherent sentences.
     - **Cluster-Based Filters**: Groups web documents using unsupervised clustering to identify semantic similarities, labels quality levels, and excludes low-quality clusters.

---

### 2. **Deduplication**
   - Multi-level deduplication to eliminate redundancy:
     - **Document-Level MinHash Deduplication**: Identifies similar documents across the corpus.
     - **Sub-Document Exact-Match Deduplication**: Removes repeated text blocks within documents.
   - Down-sampling of unhelpful content (e.g., advertisements) to ensure high information density.

---

### 3. **Tokenization**
   - **Byte Pair Encoding (BPE)**:
     - Utilizes the SentencePiece framework with a vocabulary size of 64,000 tokens.
     - Handles numeric data effectively by splitting numbers into individual digits for better comprehension.
     - Supports fallback Unicode-byte encoding to ensure robustness with rare characters.
   - Avoids positional assumptions like leading whitespace, improving performance across diverse sentence structures and languages.

---

### 4. **Data Composition**
   - **Bilingual Focus**:
     - Both English and Chinese content, offering strong performance in bilingual tasks.
   - **Balanced Sources**:
     - Includes books, academic articles, Common Crawl, curated knowledge sources, and synthetic data.
   - **Higher Deduplication Strength**: Results in cleaner datasets than pipelines like RefinedWeb or RedPajama.

---

### 5. **Finetuning Dataset**
   - **Small, High-Quality Dataset**: <10K instruction-response pairs, manually created and polished over multiple iterations.
   - **Emphasis on Diversity**:
     - Covers areas like mathematics, reasoning, creative writing, coding, safety, and bilingual tasks.
   - **Instruction Tagging System**: Balances dataset distribution across different capabilities using a diversity-focused sampling algorithm.
   - **Hallucination and Repetition Reduction**:
     - Rigorous review to ensure factual accuracy and eliminate repeated patterns in responses.

---

### 6. **Synthetic Data**
   - Used for long-context adaptation and vision-language tasks.
   - Generates high-quality, task-specific datasets like multi-document QA pairs, which help fine-tune capabilities in retrieving relevant answers from long sequences.

---

### 7. **Data Innovation Highlights**
   - **Quality over Quantity**: Prefers clean, well-structured data over raw volume, with 3T high-quality tokens outperforming unfiltered larger datasets.
   - **Bilingual Excellence**: Optimizes for strong performance in both English and Chinese.
   - **Pipeline Robustness**: Employs advanced deduplication and filtering techniques to ensure only the most relevant, safe, and coherent data is retained.

---

This meticulous data preparation strategy is central to the Yi model’s success, enabling it to achieve near GPT-3.5-level performance with fewer parameters and on consumer-grade hardware. The approach highlights a shift from relying on large-scale raw data to prioritizing data cleanliness and task-specific relevance.
