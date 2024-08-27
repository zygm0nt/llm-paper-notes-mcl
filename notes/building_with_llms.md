## Best practices

The article offers a comprehensive overview of best practices when building with large language models (LLMs). Here are the main points summarized:

1. **Prompting Best Practices**: It emphasizes the critical role of prompting in LLM applications. Key recommendations include:
   - **Utilizing n-shot prompts and in-context learning** to align the model's outputs with expectations.
   - **Chain-of-thought prompting** to enhance clarity and reduce hallucinations by making the model's reasoning steps explicit.
   - **Providing relevant resources** to the model can expand its knowledge base and improve response accuracy and trustworthiness.

2. **Input and Output Structuring**: Structured inputs help the model better understand context, while structured outputs facilitate integration into downstream systems. Examples include using schema definitions in prompts and ensuring output formats that mesh with existing systems.

3. **Simplicity in Prompts**: Encourages designing prompts that are simple and focused, capable of doing one thing well to avoid over-complication, which might degrade performance.

4. **Information Retrieval and RAG (Retrieval-Augmented Generation)**: Highlights the effectiveness of RAG in providing background knowledge which helps in grounding the LLM’s responses in reality.

5. **Workflow Optimization**: Discusses the value of breaking down complex tasks into simpler, manageable components, which can then be handled in a step-by-step workflow to enhance overall effectiveness and efficiency.

6. **Monitoring and Evaluation**: Stresses the importance of continuous evaluation and monitoring of LLM outputs to ensure accuracy and reliability. Suggests using various methods like LLM-as-Judge for comparative evaluations and execution-based evaluations for code-generation tasks.

7. **Finetuning**: While not always necessary, finetuning can be critical for tasks where basic prompting and other techniques fall short. The document provides examples of successful finetuning applications.

These best practices are aimed at optimizing the performance of LLMs in real-world applications, ensuring their outputs are reliable, accurate, and useful in various professional settings.

## Evaluation & Metrics

The article emphasizes the importance of rigorous evaluation and monitoring of large language models (LLMs) to ensure their effectiveness and reliability. Here's a summary of the key points related to evaluation and metrics:

1. **Creation of Assertion-based Unit Tests**: The authors recommend creating unit tests that include real input/output samples. These tests should validate the model's responses based on predefined criteria, ensuring that changes to the pipeline maintain output quality.

2. **Use of LLM-as-Judge**: LLM-as-Judge involves using one LLM to evaluate the output of another. This method can be effective for making comparative evaluations between control and treatment outputs. It's recommended to use pairwise comparisons and control for biases like position and response length.

3. **Intern Test**: This is a conceptual evaluation method where you consider whether a typical intern could successfully complete the task given the same inputs provided to the LLM. This helps assess the practicality and complexity of tasks assigned to the LLM.

4. **Overemphasis on Certain Metrics**: The authors caution against overemphasizing specific metrics like the Needle-in-a-Haystack (NIAH) eval, which might not reflect real-world applicability and could skew development efforts away from practical improvements.

5. **Simplifying Annotation Tasks**: To reduce variability and enhance the reliability of human judgments, the article suggests simplifying annotation tasks to binary decisions or pairwise comparisons. This approach reduces cognitive load and improves the consistency of annotations.

These strategies are designed to rigorously evaluate LLMs in various contexts, ensuring that they not only perform well according to technical metrics but also provide useful and reliable outputs in practical applications.
