# 🔍 TCRR: Qualitative Case Studies
*(Anonymous Materials for ICML 2026 Submission #14880)*

Thank you for reviewing our paper. Below we provide comprehensive visual case studies to explicitly demonstrate how **Task-Conditioned Resolution Routing (TCRR)** dynamically assigns resolution area scaling ratios based on the semantic demands of different textual prompts. This mechanism aims to find the optimal Pareto balance between feature retention and computational efficiency, rather than blindly pursuing maximum compression.

---

## 🌟 Category 1: Intra-Image Prompt Sensitivity (Same Image, Different Routing)
This category demonstrates TCRR's core novelty: resolution allocation is dynamically determined by the text prompt, not just the visual complexity of the image itself. 

### 🏙️ Case 1A: 4K Urban Landscape
<p align="center">
  <img src="figures/Case_1A_3840_2523.jpg" alt="Case 1A Original" width="70%">
  <br><em>Original Resolution: 3840 x 2523</em>
</p>

| 🎯 Task Type | 📝 Prompt | ⚙️ TCRR Routing Decision | 🖼️ Actual Visual Input | 💡 Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Global Semantics** | *"Is this picture taken during the day or at night?"* | **Area Scale: 10%**<br>*(Equivalent Res: ~1214 x 798)* | <img src="figures/Case_1A_1214_798.jpg" width="300" alt="10% Scale"> | While pruning **90%** of visual tokens, the model retains sufficient global illumination features to correctly output **"Daytime"**. This is the optimal computational sweet spot for high-confidence predictions. |
| **Fine Details** | *"What is the text inside the red rectangular area on the building in the bottom center?"* | **Area Scale: 100%**<br>*(Original Res: 3840 x 2523)* | <img src="figures/Case_1A_3840_2523.jpg" width="300" alt="100% Scale"> | The model dynamically determines that pixel-level precision is required. It bypasses down-sampling to accurately read: **"MUZIUM TEKSTIL NEGARA / National Textile Museum"**. |

<br>

### 📚 Case 1B: 2K Indoor Study Area
<p align="center">
  <img src="figures/Case_1B_2048_2048.jpg" alt="Case 1B Original" width="60%">
  <br><em>Original Resolution: 2048 x 2048</em>
</p>

| 🎯 Task Type | 📝 Prompt | ⚙️ TCRR Routing Decision | 🖼️ Actual Visual Input | 💡 Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Global Semantics** | *"What is the functional scene of this room?"* | **Area Scale: 10%**<br>*(Equivalent Res: ~648 x 648)* | <img src="figures/Case_1B_648_648.jpg" width="300" alt="10% Scale"> | After aggressively removing **90%** of local redundancy, the model relies on core structural features to correctly output: **"Computer lab or library reading area"**. |
| **Fine Details** | *"What brands are the computers on the desk?"* | **Area Scale: 90%**<br>*(Equivalent Res: ~1943 x 1943)* | <img src="figures/Case_1B_1943_1943.jpg" width="300" alt="90% Scale"> | TCRR intelligently assigns a high resolution to extract micro-features (the tiny logo on the back of the distant Dell monitor would blur entirely at low resolutions). Output: **"Apple and Dell"**. |

---

## 📊 Category 2: High-Resolution Retention for Dense Information
For images with dense text or complex structures (e.g., charts, documents), TCRR conservatively maintains high pixel retention rates to prevent the loss of critical fine-grained structures during global compression.

### 📉 Case 2A: Chart QA (ChartQA)
<p align="center">
  <img src="figures/Case_2A_800_836.jpg" alt="Case 2A Original" width="50%">
  <br><em>Original Resolution: 800 x 836</em>
</p>

| 🎯 Task Type | 📝 Prompt | ⚙️ TCRR Routing Decision | 🖼️ Actual Visual Input | 💡 Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Information Extraction** | *"Which airport was the most used?"* | **Area Scale: 80%**<br>*(Equivalent Res: ~716 x 748)* | <img src="figures/Case_2A_716_748.jpg" width="300" alt="80% Scale"> | Recognizing dense axis texts and fine-grained data bars, TCRR adopts an extremely conservative down-sampling strategy. It marginally reduces resolution to save 20% compute while preserving impeccable legibility, correctly outputting: **"Paris-Charles-de-Gaulle"**. |

<br>

### 📄 Case 2B: Dense Document QA (DocVQA)
<p align="center">
  <img src="figures/Case_2B_1679_2340.jpg" alt="Case 2B Original" width="50%">
  <br><em>Original Resolution: 1679 x 2340</em>
</p>

| 🎯 Task Type | 📝 Prompt | ⚙️ TCRR Routing Decision | 🖼️ Actual Visual Input | 💡 Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Document Parsing** | *"What is the % of raw material imported in the current year?"* | **Area Scale: 90%**<br>*(Equivalent Res: ~1593 x 2220)* | <img src="figures/Case_2B_1593_2220.jpg" width="300" alt="90% Scale"> | To extract a specific percentage from a financially dense table, TCRR assigns near-maximum resolution (90%). This strictly guarantees that microscopic digits do not suffer from aliasing before entering the ViT, enabling the model to precisely extract: **"79.23%"**. |

---

## ⚡ Category 3: Extreme Redundancy Pruning
For high-level semantic queries (e.g., mood perception or basic object recognition), fine-grained details are computationally inefficient. TCRR aggressively scales down the entire image (pruning 80%-90% of pixel area) to extract core semantics at ultra-low latency.

### 🌄 Case 3A: Landscape (Mood Perception)
<p align="center">
  <img src="figures/Case_3A_580_381.jpg" alt="Case 3A Original" width="50%">
  <br><em>Original Resolution: 580 x 381</em>
</p>

| 🎯 Task Type | 📝 Prompt | ⚙️ TCRR Routing Decision | 🖼️ Actual Visual Input | 💡 Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Mood Perception** | *"What is the overall mood or atmosphere conveyed by this landscape?"* | **Area Scale: 20%**<br>*(Equivalent Res: ~259 x 170)* | <img src="figures/Case_3A_259_170.jpg" width="250" alt="20% Scale"> | Pixel-level textures (individual leaves, tiny ripples) are redundant for mood perception. TCRR aggressively scales the image down to 20% to capture global color blocks and contours, accurately predicting: **"Serene and natural"** at ultra-low latency. |

<br>

### 🐕 Case 3B: Object Recognition (Salient Subject)
<p align="center">
  <img src="figures/Case_3B_680_454.jpg" alt="Case 3B Original" width="50%">
  <br><em>Original Resolution: 680 x 454</em>
</p>

| 🎯 Task Type | 📝 Prompt | ⚙️ TCRR Routing Decision | 🖼️ Actual Visual Input | 💡 Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Basic Recognition** | *"What is the main animal lying right in the center?"* | **Area Scale: 10%**<br>*(Equivalent Res: ~215 x 144)* | <img src="figures/Case_3B_215_144.jpg" width="250" alt="10% Scale"> | Iconic global features of a Shiba Inu (reddish-gold coat, triangular ears, silhouette) remain distinctly recognizable even when uniformly compressed to 10% of the original area. TCRR assigns the lowest resolution routing, maximizing efficiency while fully preserving semantic accuracy: **"Shiba Inu"**. |
