# Summary

The document introduces "phi-3-mini," a compact 3.8 billion-parameter language model designed to run locally on mobile devices while delivering performance comparable to larger models like GPT-3.5. It achieves this by utilizing heavily curated web data and synthetic data during training, focusing on optimizing the quality of the training data to maintain high performance. The model supports advanced reasoning, multilingual tasks, and long-context capabilities, with post-training focused on improving safety and alignment for responsible AI use.

## Model versions

Here’s an outline of the key parameters for the phi-3 models mentioned in the document:

### Phi-3-Mini:
- **Parameters**: 3.8 billion
- **Training Tokens**: 3.3 trillion
- **Context Length**: 4K (standard), 128K (via LongRope for extended context)
- **Hidden Dimension**: 3072
- **Heads**: 32
- **Layers**: 32
- **Tokenizer Vocabulary**: 320641
- **Quantized Size**: Can be quantized to 4-bits, occupying ~1.8GB of memory

### Phi-3-Small:
- **Parameters**: 7 billion
- **Training Tokens**: 4.8 trillion
- **Context Length**: 8192
- **Hidden Dimension**: 4096
- **Heads**: 32
- **Layers**: 32
- **Tokenizer Vocabulary**: 1003522 (uses tiktoken tokenizer for better multilingual tokenization)
- **Special Features**: Incorporates GEGLU activation and Maximal Update Parametrization (muP) for better training stability, and uses blocksparse attention for memory efficiency.

### Phi-3-Medium:
- **Parameters**: 14 billion
- **Training Tokens**: 4.8 trillion (similar to Phi-3-Small but with more epochs)
- **Context Length**: 8192
- **Hidden Dimension**: 5120
- **Heads**: 40
- **Layers**: 40
- **Tokenizer Vocabulary**: 320641 (same as Phi-3-Mini and Phi-3-Small)
  
These models differ mainly in size (parameters), hidden dimensions, and the number of layers and heads, with the phi-3-small and medium models trained for more tokens to support more complex tasks.

## Fine-tuning

The fine-tuning phase for the **phi-3 series models** involves multiple stages aimed at optimizing the models for safety, performance, and alignment with user interaction needs. Here's a breakdown of the key components of the fine-tuning process:

### 1. **Supervised Fine-Tuning (SFT)**:
   - **Objective**: SFT uses curated, high-quality datasets to further train the model in a more controlled environment after the initial pre-training.
   - **Data Mix**: The datasets include examples from diverse domains such as:
     - **Mathematics**
     - **Coding**
     - **Reasoning**
     - **General conversation**
     - **Model identity and safety**
   - **Language Focus**: The process initially focuses on **English-only examples** to refine general language understanding.
   - **Chat-Finetuning**: The models are also fine-tuned specifically for **chat formats**, ensuring they can handle dialogue-based interactions with users more effectively.

### 2. **Direct Preference Optimization (DPO)**:
   - **Objective**: DPO aims to refine the model’s behavior by teaching it to avoid unwanted outputs. This is accomplished by providing "rejected" responses during fine-tuning, helping the model understand what kind of answers should be avoided.
   - **Focus Areas**:
     - **Responsible AI (RAI)**: Efforts are made to align the model to produce safe and ethical responses, reducing the chances of generating harmful or biased outputs.
     - **Reasoning and Robustness**: The model’s reasoning capabilities and robustness are enhanced through specialized datasets targeting logical and common-sense reasoning.

### 3. **Safety Alignment**:
   - **Post-Training Safety**: After the SFT and DPO phases, further alignment takes place to ensure the model is safe for public use. This involves:
     - **Red-Teaming**: Independent teams test the model to expose vulnerabilities or problematic behaviors.
     - **Harm Categories**: The model is evaluated across several harm categories, and additional datasets are created to address any issues found during red-teaming.
   
### 4. **Outcome of Fine-Tuning**:
   - **Improved Performance**: The fine-tuning process significantly boosts the model’s performance across reasoning, coding, math, and safety-related benchmarks.
   - **AI Assistant Transformation**: Post-fine-tuning, the model becomes capable of efficiently handling diverse tasks in a user-friendly, chat-oriented format, making it an effective AI assistant.

Overall, fine-tuning focuses on improving both the technical performance of the models (in terms of reasoning and tasks) and ensuring alignment with responsible AI principles to minimize harmful or inappropriate behavior.

## How do Phi-3 models compare against Orca models?

The comparison between **phi-3 models** and **Orca models** (developed by Microsoft as well) can be framed around several key aspects such as model size, performance, fine-tuning methodologies, training data, and deployment capability. Here's an overview of how these models stack up against each other:

### 1. **Model Size and Architecture**:
   - **Phi-3 Models**: The phi-3 series has models of varying sizes:
     - **phi-3-mini** (3.8B parameters)
     - **phi-3-small** (7B parameters)
     - **phi-3-medium** (14B parameters)
   - **Orca Models**: Orca is also a family of models, and typically references models with larger parameter sizes. Orca models are often positioned as capable of handling complex, high-level reasoning tasks due to their larger scale and focus on advanced distillation techniques.
     - Commonly mentioned models in the Orca family are around 13B to 30B parameters, putting them closer to **phi-3-medium** in terms of scale.

### 2. **Training Data and Distillation**:
   - **Phi-3 Models**: The phi-3 models, particularly phi-3-mini, focus on optimizing the quality of the training data using heavily filtered public web data and synthetic data. This approach allows them to achieve performance on par with larger models despite their smaller parameter count.
   - **Orca Models**: Orca models emphasize a **teacher-student learning framework** (distillation approach), where smaller models learn from the reasoning traces of large, sophisticated models like GPT-4. This allows Orca models to exhibit advanced reasoning capabilities by imitating the behavior of much larger models through multi-step reasoning distillation.
     - Orca’s focus on "student" models learning from the knowledge of larger models (like GPT-4) allows for high performance on reasoning tasks with fewer parameters.

### 3. **Performance on Benchmarks**:
   - **Phi-3 Models**: Phi-3-mini is comparable in performance to GPT-3.5 and Mixtral 8x7B, achieving strong results on benchmarks like **MMLU** (69% for phi-3-mini, 75-78% for phi-3-small and medium) and **MT-bench** (8.38 for phi-3-mini, up to 8.9 for phi-3-medium).
   - **Orca Models**: Orca models tend to excel at reasoning-intensive tasks, which is a result of their fine-tuning with reasoning traces. Orca models generally outperform smaller models that do not use the distillation approach. Orca models also tend to perform better than regular LLaMA models on benchmarks like **MMLU** and **HellaSwag**.

### 4. **Multimodal Capabilities**:
   - **Phi-3.5-Vision**: The phi-3.5 series extends into multimodal territory with the phi-3.5-Vision model, which can process both images and text. This gives phi-3 models an edge in tasks involving vision and language understanding.
   - **Orca Models**: Orca models, as of now, are typically focused on pure language reasoning tasks and do not specifically target multimodal tasks like phi-3.5-Vision.

### 5. **Deployment Efficiency**:
   - **Phi-3 Models**: One of the primary innovations of the phi-3-mini model is its ability to run on **mobile devices** thanks to its smaller size (3.8B parameters) and 4-bit quantization, making it highly accessible for local deployment. This makes the phi-3-mini an efficient option for edge computing.
   - **Orca Models**: Orca models are generally larger and more computationally intensive, making them better suited for cloud or server-based deployments. They are not designed for local or on-device deployment in the same way as phi-3-mini.

### 6. **Fine-Tuning and Safety**:
   - **Phi-3 Models**: Fine-tuning for phi-3 includes **Supervised Fine-Tuning (SFT)** and **Direct Preference Optimization (DPO)**, with a strong focus on responsible AI and safety. Post-training safety alignment is critical for these models, including **red-teaming** to mitigate harmful outputs.
   - **Orca Models**: Orca models also leverage fine-tuning techniques, especially with teacher-student distillation, but are less focused on the extensive safety testing and fine-tuning for responsible AI compared to phi-3 models.

### Summary of Comparison:
- **Model Size**: Orca models are generally larger but comparable to phi-3-medium in scale.
- **Training and Learning**: Orca models use reasoning trace distillation, while phi-3 focuses on data quality optimization.
- **Deployment**: Phi-3-mini is optimized for mobile deployment, which Orca models are not.
- **Performance**: Both perform well on benchmarks, but Orca models tend to excel in reasoning tasks due to distillation from larger models.
- **Multimodal Capabilities**: Phi-3.5-Vision offers multimodal features, while Orca does not.
- **Fine-Tuning and Safety**: Both models are fine-tuned, but phi-3 models have a more robust safety alignment process.

Overall, phi-3 models are more compact and versatile in terms of deployment, while Orca models focus on superior reasoning performance through teacher-student learning and are more aligned with server-based deployments.
