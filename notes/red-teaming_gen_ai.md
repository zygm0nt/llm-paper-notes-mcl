**Summary of "Lessons From Red Teaming 100 Generative AI Products" (Microsoft, 2025)**

This paper presents insights and recommendations from Microsoft's AI Red Team (AIRT), which has red teamed over 100 generative AI (GenAI) products. It introduces a comprehensive threat model ontology and outlines eight key lessons, emphasizing the evolving nature of AI risks and the importance of both technical and human-centric approaches in red teaming.

---

### 🔍 **Key Takeaways**

#### **AIRT Threat Model Ontology**
- **System**: AI product under test
- **Actor**: Adversarial or benign user
- **TTPs**: Tactics, Techniques, and Procedures (mapped to MITRE ATT&CK/ATLAS)
- **Weakness**: Exploitable flaws
- **Impact**: Resulting harm or breach
- **Mitigation**: Measures to fix vulnerabilities

---

### 🧠 **Eight Key Lessons**

1. **Understand the system’s capabilities and application context**
   - Align testing with real-world impacts.
   - Larger models pose different risks (e.g., advanced encoding attacks).

2. **Simple attacks are often more practical than gradient-based ones**
   - Prompt engineering > complex adversarial ML in real-world scenarios.
   - Attack entire systems, not just models.

3. **Red teaming ≠ safety benchmarking**
   - Red teaming uncovers novel, contextual, and emergent harms.
   - Benchmarks are static and often miss real-world misuse.

4. **Automation helps scale**
   - PyRIT framework enables automated attack generation and scoring.
   - Helps cover the non-deterministic behavior of GenAI models.

5. **The human element is critical**
   - Judgment, creativity, cultural and domain expertise, and emotional intelligence are irreplaceable.
   - Example: Testing chatbot responses to distressed users.

6. **Responsible AI (RAI) harms are pervasive and hard to quantify**
   - Harms can be caused by both adversarial and benign users.
   - Example: Gender bias in image generation and psychosocial harms from LLMs.

7. **LLMs amplify existing risks and introduce new ones**
   - Traditional issues (e.g., SSRF) still apply.
   - New threats include prompt injections, model manipulation, and misuse of reasoning capabilities.

8. **Securing AI is an ongoing process**
   - No system is foolproof; aim to raise the cost of attacks.
   - Use “break-fix” cycles, purple teaming, and policy/regulatory support.

---

### 🚨 **Top Attack Vectors Identified**

| **Vector**                        | **Description**                                                                 |
|----------------------------------|---------------------------------------------------------------------------------|
| **Prompt Injection / XPIA**      | Hidden instructions within prompts or inputs hijack model behavior.             |
| **Jailbreaks (text/image)**      | Malicious prompts (e.g., overlaid in images) bypass safety filters.             |
| **Multimodal Exploits**          | Combined text + image prompts to elicit unsafe or illegal content.              |
| **Automated Scamming**           | LLMs + TTS/STT used to create end-to-end scam systems.                          |
| **SSRF / Dependency Exploits**   | Outdated libraries (e.g., FFmpeg) used in AI systems can enable network attacks.|
| **Model Bias / Stereotyping**    | Biases emerge even in benign usage (e.g., text-to-image gender stereotyping).   |
| **System-Level Recon & Execution**| Attackers combine prompt injection with code execution for data exfiltration.   |
| **Roleplay-Based Exploits**      | Users simulate distress or other roles to bypass safeguards.                    |
| **Language-Based Gaps**          | Jailbreaks or harmful behaviors vary across multilingual contexts.              |

---

Let me know if you want:
- A visual threat matrix
- Deeper dive into PyRIT
- Practical checklist for AI red teaming
