# Tactical

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

# Operational

## Monitoring Data

In the article, monitoring data effectively involves several key practices when working with large language models (LLMs). Here are some highlighted approaches:

1. **Review LLM Inputs and Outputs Regularly**: Engage with the data regularly to understand its distribution, identify patterns, and monitor for changes. This hands-on approach helps in catching issues early and adjusting strategies accordingly.

2. **Check for Development-Production Skew**: It’s important to ensure that the data used in development closely mirrors the data encountered in production. Differences in data formatting or content can lead to reduced accuracy and performance when the model is deployed.

3. **Measure Skew Systematically**: Implement systematic measurements of skew in the data, including structural differences (like formatting errors) and content-based discrepancies. Use metrics like input-output length or specific formatting requirements to track changes over time.

4. **Use Advanced Drift Detection Techniques**: Consider using advanced techniques such as clustering embeddings of input-output pairs to detect semantic drifts in topics or user behavior that might indicate areas the model has not been exposed to before.

5. **Perform Regular "Vibe Checks"**: Regularly review outputs to ensure they meet quality standards and align with user expectations. This can involve qualitative assessments alongside quantitative measures.

6. **Incorporate Feedback Mechanisms**: Enable easy feedback mechanisms for users to report errors or provide corrections. This human-in-the-loop approach not only improves model output but also gathers valuable data for further refinement.

These strategies are crucial for maintaining the reliability and accuracy of LLM applications, ensuring they continue to perform well in real-world conditions.

## Product development

According to the second article in the series "What We Learned from a Year of Building with LLMs," working with developed products that utilize large language models (LLMs) involves a combination of operational, strategic, and team-focused approaches to ensure the products are effective, efficient, and continuously improving. Here’s a detailed guide on how to work with these developed products based on insights from the article:

### 1. **Operational Management**
   - **Data Monitoring**: Regularly check and analyze the data being used by the LLMs for both input and output. This includes looking for skew between development and production data to ensure consistency and reliability.
   - **Model Integration**: Seamlessly integrate LLMs into existing tech stacks, ensuring that model updates and versioning are handled without disrupting the product's functionality.
   - **Performance Evaluation**: Continuously monitor the performance of the LLMs in terms of accuracy, speed, and reliability. Adjust and optimize the models as needed based on performance data.

### 2. **Strategic Implementation**
   - **Feedback Loops**: Implement mechanisms for users to provide feedback on the product. Use this feedback to refine the model and address any issues or shortcomings identified by users.
   - **Iterative Improvement**: Employ an agile development approach, where the product is continuously updated and improved based on user feedback and new advancements in LLM technology.
   - **Risk Management**: Understand and manage the risks associated with LLM outputs, especially in terms of accuracy and appropriateness of the content generated by the models.

### 3. **User Experience Design**
   - **Human-in-the-Loop (HITL)**: Design the product such that there is human oversight on critical outputs, especially in areas where LLMs may generate sensitive or impactful content.
   - **UX Optimization**: Work closely with UX/UI designers to ensure that the product interface is user-friendly and that the integration of LLM outputs is seamless and intuitive for users.

### 4. **Team Collaboration and Skills Development**
   - **Cross-Functional Teams**: Encourage collaboration between AI/ML engineers, product managers, UX designers, and data scientists to ensure that all aspects of the product are aligned and optimized.
   - **Training and Development**: Provide ongoing training for team members on the latest LLM technologies and best practices in AI ethics and security.

### 5. **Quality Assurance and Regulatory Compliance**
   - **Continuous Testing**: Implement continuous testing processes to ensure that updates to the LLMs or the product do not introduce new bugs or degrade the product’s performance.
   - **Compliance Checks**: Regularly review and update the product to ensure compliance with relevant laws and regulations, particularly those related to data privacy and AI ethics.

### 6. **Market Adaptation**
   - **Market Feedback**: Actively seek out and analyze user feedback and market trends to understand how the product is being used and what new features or improvements are needed.
   - **Adaptive Strategies**: Be prepared to pivot or adapt the product strategy based on feedback and changes in the market landscape to stay competitive and relevant.

By following these guidelines, you can effectively manage and enhance LLM-based products, ensuring they not only meet the current needs of users but are also well-positioned to adapt to future changes and challenges in technology and market demands. The article emphasizes a holistic approach, balancing technical requirements with strategic planning and team dynamics to maximize the potential of LLM technologies in developed products.

## Team structure

According to the second article in the series "What We Learned from a Year of Building with LLMs," structuring teams around LLM-based products involves a focus on various operational aspects, such as data, models, product, and people. The article provides detailed guidance on setting up a team to manage each of these areas effectively. Here’s a summary of how to structure teams based on the insights from the article:

### 1. **Operations: Developing and Managing LLM Applications and the Teams That Build Them**
   - **Data Team**: Focuses on monitoring the inputs and outputs of the LLMs, checking for development-production skew, and regularly reviewing samples to ensure the data quality and relevance are maintained. This team should include data scientists and data engineers.
   - **Models Team**: Manages the integration and versioning of LLMs within the product stack. This includes software engineers specialized in AI/ML, who work on integrating language models and handling migrations between different model versions or updates.
   - **Product Team**: Led by product managers, this team works on the design and user experience aspects, focusing on how the LLM outputs are utilized in the product and ensuring that the product meets the functional requirements of the users.
   - **People Team**: Focuses on the human aspects, such as hiring the right talent, fostering a culture of experimentation, and training team members to work effectively with LLM technologies.

### 2. **Key Roles Within the Teams**
   - **AI/ML Engineers**: Specialize in tweaking and fine-tuning the LLMs for specific tasks.
   - **Data Scientists**: Analyze and preprocess data to feed into the LLMs, and also interpret the outputs from these models.
   - **Software Engineers**: Develop the infrastructure and APIs to integrate LLMs with the product’s other components.
   - **Product Managers**: Oversee the project's progress, from ideation to launch, ensuring the product meets market needs.
   - **UX/UI Designers**: Ensure that the application interface is intuitive and that the interactions with the LLM are seamless for the end-user.
   - **Ethics and Compliance Officers**: Address potential ethical issues and ensure the product complies with relevant laws and regulations.

### 3. **Cross-Functional Collaboration**
   - Encourage regular interaction between these teams to ensure alignment and facilitate the exchange of ideas. This can be achieved through regular meetings, shared projects, and cross-training sessions.

### 4. **Focus on Continuous Learning**
   - Given the rapid evolution of LLM technologies, continuous learning and adaptation are crucial. Provide opportunities for team members to learn about the latest developments in the field and incorporate this knowledge into their work.

### 5. **Empower Teams with the Right Tools**
   - Equip teams with the necessary tools and technologies to efficiently manage the complexities of working with LLMs, such as version control systems for models, monitoring tools for performance tracking, and platforms for collaborative development.

### 6. **Iterative Development and Feedback Loops**
   - Adopt an agile methodology that allows for iterative development and incorporates feedback from all stakeholders, including end-users. This approach helps in rapidly addressing issues and adapting the product based on real-world usage and feedback.

By organizing teams around these functional areas and fostering a culture of collaboration and continuous improvement, companies can effectively develop, deploy, and manage LLM-based products. The article underscores the importance of not just the technical aspects but also the operational, product, and people management aspects, which are critical to the success of LLM initiatives.
