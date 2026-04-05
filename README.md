# TCRR: Qualitative Case Studies 
*(Anonymous Materials for ICML 2026 Submission #14880)*

Thank you for reviewing our paper. Below we provide visual case studies to explicitly demonstrate how Task-Conditioned Resolution Routing (TCRR) dynamically assigns resolution scales based on the semantic demands of different text prompts.

---

## Case 1: Intra-Image Prompt Sensitivity (Global vs. Local)
This case demonstrates how TCRR handles the **exact same 4K image** when given different textual queries.

**Image 1: Global Semantic Query**
> **Prompt:** "Is it raining?"
> **TCRR Routing:** Scale 10% (Aggressive Compression)
> **Output:** "No, it is sunny." 
*(Explanation: The router correctly identifies that answering a global weather question requires minimal visual details, saving 90% of FLOPs.)*

![Case 1 Global](case1_global.png)

**Image 2: Fine-Grained Query**
> **Prompt:** "Read the license plate of the blue car."
> **TCRR Routing:** Scale 100% (Zero Compression)
> **Output:** "XYZ-1234"
*(Explanation: The router intelligently perceives the need for pixel-level precision and dynamically allocates maximum resolution.)*

![Case 1 Local](case1_local.png)

---

## Case 2: Document and Chart Adaptation
For text-dense images, TCRR maintains high resolution to preserve structural integrity.

> **Prompt:** "What is the 2023 revenue according to this chart?"
> **TCRR Routing:** Scale 80%
> **Output:** "$1.2M"

![Case 2 Chart](case2.png)

---

## Case 3: Extreme Redundancy Pruning
For highly abstract or general visual queries, TCRR maximizes efficiency.

> **Prompt:** "What is the overall mood of this painting?"
> **TCRR Routing:** Scale 20%
> **Output:** "Melancholy."

![Case 3 Art](case3.png)
