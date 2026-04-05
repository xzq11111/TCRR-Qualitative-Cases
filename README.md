# TCRR: Qualitative Case Studies (Anonymous for ICML 2026)

This repository contains illustrative examples of the routing preference patterns learned by Task-Conditioned Resolution Routing (TCRR).

## Visual Case Studies
As shown in the image below, TCRR dynamically adjusts the resolution scale based strictly on the semantic demands of the textual prompt, even for the exact same image.

![TCRR Cases](cases.png)

* **Case 1 (Top):** Demonstrates that TCRR assigns completely different resolutions (10% vs 100%) to the *same* image based on whether the prompt asks for global gist or fine-grained details.
* **Case 2 (Middle):** Shows TCRR intelligently retaining high resolution (80%) for text-dense document tasks.
* **Case 3 (Bottom):** Shows TCRR aggressively pruning resolution (20%) for global semantic tasks.
