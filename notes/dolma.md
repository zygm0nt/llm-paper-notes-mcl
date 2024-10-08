## Summary

The paper titled **"Dolma: an Open Corpus of Three Trillion Tokens for Language Model Pretraining Research"** discusses the development and release of a large-scale corpus, **Dolma**, designed to support the training of language models. Here is a summary of the key points:

### **Purpose**
- The main motivation for creating Dolma is to facilitate scientific research in pretraining language models by providing an open and transparent corpus, contrasting with many commercial models that do not disclose their training data.
- Dolma aims to increase understanding of how the composition of pretraining data impacts the capabilities and limitations of language models.

### **Content of Dolma**
- Dolma consists of **three trillion tokens** and is built from a diverse range of text sources, including web content (from Common Crawl), scientific papers (from Semantic Scholar), code (from GitHub), public domain books (Project Gutenberg), social media posts (Reddit), and encyclopedic materials (Wikipedia and Wikibooks).
- The corpus has been extensively cleaned and curated to make it suitable for training large language models.

### **Key Contributions**
1. **Release of the Dolma Corpus**: Dolma provides a large pool of pretraining data, offering transparency and accessibility to the public. The corpus is accompanied by detailed documentation about its creation, curation, and filtering processes.
2. **Open-Source Toolkit**: Alongside the corpus, the authors release a **data curation toolkit**, which is designed to help others replicate the dataset and curate their own large-scale corpora for language models.

### **Data Curation Practices**
- **Deduplication**: Deduplication of documents and paragraphs ensures that repetitive information does not overly bias the model's learning process.
- **Content Filtering**: Toxic or inappropriate content is filtered out using machine learning classifiers trained on datasets like Jigsaw's Toxic Comments. Personal identifiable information (PII) is masked or removed.
- **Data Ablations**: Experimental results on intermediate versions of Dolma help analyze the effects of data quality and content on downstream model performance.

### **Research and Transparency**
- **Scientific Inquiry**: Dolma’s open nature allows researchers to investigate how different pretraining data compositions affect model behavior, including the exploration of biases, memorization, and data contamination.
- **Open Pretraining Data**: By openly sharing Dolma, the authors aim to foster better research on the interactions between language models and their training data, addressing the opacity of commercial language models like GPT-4 and Claude.

### **Impact**
- Dolma offers a **larger token size** than previous open datasets like C4 or The Pile, while maintaining a diverse range of data sources.
- The corpus is expected to contribute significantly to advancing language model research, particularly by offering more insights into data curation practices and their influence on model outcomes.

In summary, **Dolma** serves as a critical resource for the research community, providing both data and tools to enable large-scale pretraining of language models, with a focus on transparency and reproducibility.

![funny](dolma_01.png)

## Dolma curation process

The curation process for **Dolma**, a large-scale corpus designed for pretraining language models, is highly detailed and involves several stages to ensure data quality, diversity, and appropriateness for language model training. Here's a breakdown of the key steps involved in curating Dolma:

### 1. **Data Sources**
Dolma was created by aggregating data from six major sources:
   - **Common Crawl (web data)**: Includes a variety of general web content.
   - **GitHub (code data)**: Extracted from open-source repositories.
   - **Reddit (social media data)**: Comments and posts from Reddit's public API.
   - **Semantic Scholar (scientific papers)**: Open access papers from the Semantic Scholar Open Research Corpus (S2ORC).
   - **Project Gutenberg (books)**: Public domain books, mostly published before 1927.
   - **Wikipedia/Wikibooks (encyclopedic content)**: English-language content from Wikipedia and Wikibooks.

### 2. **Language Filtering**
   - **Language Identification**: FastText classifiers are used to identify English content. Documents with an English score of less than 0.5 are discarded.
   - This filtering is applied across all subsets of Dolma to ensure the corpus contains primarily English-language documents.

### 3. **Quality Filtering**
Quality filtering is implemented to remove low-quality or non-prose-like content that would be unhelpful or distracting during model pretraining:
   - **HTML Artifacts Removal**: For web data, conversion artifacts like headers, navigation elements, and non-textual content (e.g., ads, metadata) are removed.
   - **Gopher-Like Rules**: Rules are applied to flag documents that contain unusual formatting (e.g., repeated characters, strange symbols, high ratio of digits or special characters).
   - **Length-Based Filtering**: Documents with fewer than 50 words or overly long documents (>100,000 characters) are excluded.
   - **Heuristic Filtering (Gopher Rules)**: Rules from other datasets, like Gopher and C4, are applied to remove documents that are not prose-like (e.g., documents without punctuation at the end of sentences).

### 4. **Content Filtering**
Content filtering is crucial for removing inappropriate, toxic, or sensitive content:
   - **Toxicity Filtering**: Classifiers trained on the Jigsaw Toxic Comments dataset identify and remove toxic, hateful, or obscene language. Two thresholds are used: a "low" threshold to capture even mildly toxic content and a "high" threshold for stricter filtering.
   - **NSFW (Not Safe For Work) Content**: Filters also remove content deemed inappropriate (e.g., sexually explicit material).
   - **Personal Identifiable Information (PII)**: Regular expressions detect and remove PII such as email addresses, phone numbers, and IP addresses. Documents with too many PII instances (e.g., six or more) are removed entirely, while those with fewer PII instances are masked (replacing sensitive information with placeholder tokens).

### 5. **Deduplication**
   - **URL-Based Deduplication**: For web data, exact duplicates are removed based on the URL to ensure that the same content isn’t repeated multiple times.
   - **Document-Level Deduplication**: Documents that are identical, even across different URLs, are flagged and removed.
   - **Paragraph-Level Deduplication**: For web and Reddit data, paragraphs within documents are deduplicated using Bloom filters to prevent the repetition of identical content, such as repeated comments or boilerplate text.

### 6. **Mixing and Upsampling**
   - After curation, the sources are mixed together, ensuring that various types of content (e.g., code, web, books) are represented proportionally. Some subsets, like encyclopedic content and scientific papers, may be **upsampled** to ensure better coverage of knowledge-rich domains.

### 7. **Documentation and Transparency**
   - Extensive documentation accompanies Dolma’s release. Each data source, filtering step, and transformation is documented to provide researchers with insights into how the data was curated, offering full transparency and encouraging reproducibility.

### 8. **Decontamination**
   - **Test Set Decontamination**: To avoid contamination with downstream evaluation datasets, Dolma checks for overlaps between its training data and popular NLP benchmarks (e.g., datasets from GLUE, SuperGLUE, and others). Any overlapping data is flagged and removed.
   - **Exact Match Checks**: Paragraphs are checked for exact matches with test data from evaluation benchmarks to ensure that models trained on Dolma do not memorize test sets.

### 9. **Reproducibility and Toolkit**
   - The authors released the **Dolma Toolkit**, which includes all the tools necessary to replicate the curation process. This includes scripts for filtering, deduplication, and content classification, enabling other researchers to use similar pipelines for their own datasets.

### Summary of the Curation Process:
- **Language filtering** ensures the dataset is primarily in English.
- **Quality filtering** eliminates low-quality, non-informative, or improperly formatted documents.
- **Content filtering** removes toxic, inappropriate, and sensitive information, especially PII.
- **Deduplication** prevents duplicate documents, paragraphs, and content across different subsets.
- **Decontamination** checks against test sets to avoid contamination in evaluation.
- **Mixing** of different data sources ensures a balanced dataset suitable for pretraining.

This careful curation process helps make Dolma an ideal corpus for pretraining large language models while ensuring the dataset is clean, diverse, and suitable for a variety of research and applications.
