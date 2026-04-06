# TCRR: Qualitative Case Studies 
*(Anonymous Materials for ICML 2026 Submission #14880)*

Thank you for reviewing our paper. Below we provide comprehensive visual case studies to explicitly demonstrate how Task-Conditioned Resolution Routing (TCRR) dynamically assigns resolution scales based on the semantic demands of different text prompts.

---

## Category 1: Intra-Image Prompt Sensitivity (Same Image, Different Routing)
This category demonstrates TCRR's core novelty: resolution is determined by the text prompt, not just the image complexity.

**Case 1A: 4K Street Scene**
* **Prompt 1 (Global Gist):** "Is it raining?" $\rightarrow$ **TCRR Scale: 10%**. (Saves 90% FLOPs while correctly answering "No, it is sunny").
* **Prompt 2 (Fine Details):** "Read the license plate of the blue car." $\rightarrow$ **TCRR Scale: 100%**. (Retains full pixel-level precision to correctly read "XYZ-1234").
![Case 1A](case1A.png)

**Case 1B: Indoor Scene**
* **Prompt 1 (Global Gist):** "Is the room messy?" $\rightarrow$ **TCRR Scale: 20%**. 
* **Prompt 2 (Fine Details):** "What brand is the TV on the wall?" $\rightarrow$ **TCRR Scale: 90%**.
![Case 1B](case1B.png)

---

## Category 2: High-Resolution Retention for Dense Information
For text-dense or structurally complex images, TCRR conservatively maintains high resolutions to prevent performance drops.

**Case 2A: Financial Chart (ChartQA)**
* **Prompt:** "What is the 2023 revenue according to this chart?"
* **TCRR Routing: Scale 80%** $\rightarrow$ Output: "$1.2M". (Intelligently preserves high-frequency text integrity while still shaving off 20% redundant compute).
![Case 2A](case2A.png)

**Case 2B: Dense Document (DocVQA)**
* **Prompt:** "What is the shipping address on the invoice?"
* **TCRR Routing: Scale 90%** $\rightarrow$ Output: correctly extracts the address.
![Case 2B](case2B.png)

---

## Category 3: Extreme Redundancy Pruning
For highly abstract, simple, or global visual queries, TCRR maximizes efficiency by aggressively down-sampling.

**Case 3A: Artistic Painting**
* **Prompt:** "What is the overall mood of this painting?"
* **TCRR Routing: Scale 20%** $\rightarrow$ Output: "Melancholy." (Semantic essence is captured despite 80% visual token reduction).
![Case 3A](case3A.png)

**Case 3B: Object Recognition**
* **Prompt:** "What animal is sleeping on the grass?"
* **TCRR Routing: Scale 10%** $\rightarrow$ Output: "A golden retriever." 
![Case 3B](case3B.png)
