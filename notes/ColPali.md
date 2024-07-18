The article "ColPali: Efficient Document Retrieval with Vision Language Models" discusses the development of ColPali, a new architecture for document retrieval systems that integrates Vision Language Models (VLMs). It addresses the challenge of document retrieval systems that are traditionally strong in text-based query matching but struggle with utilizing visual cues in documents effectively. This inefficiency is particularly problematic in applications like Retrieval Augmented Generation (RAG).

The ColPali system is designed to improve retrieval performance by using VLMs to understand and index documents based on their visual elements (like images, layouts, and fonts) instead of just text. This approach allows ColPali to create high-quality, contextualized embeddings from images of document pages, enabling faster and more accurate document retrieval compared to traditional methods.

The paper also introduces the Visual Document Retrieval Benchmark (ViDoRe), which evaluates systems on their ability to retrieve documents using both textual and visual cues. ColPali significantly outperforms existing document retrieval systems in this benchmark, offering faster performance and being end-to-end trainable.

Moreover, the article details the experimental setup, model training, and the specific advantages of integrating visual features in document retrieval, showing that ColPali not only enhances retrieval accuracy but also speeds up the process due to its efficient handling of visual data.

![](../assets/colpali-diagram.png)

![](../assets/colpali-metrics.png)
