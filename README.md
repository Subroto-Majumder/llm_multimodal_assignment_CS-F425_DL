# Multimodal AI Architectures Assignment

## Overview
This assignment explores and compares three distinct architectural approaches for building Multimodal AI systems capable of processing and generating mixed media. The goal is to understand the structural trade-offs between pipelined systems, adapter-based integration, and unified native models.

## Implemented Approaches

### Path 1: LLM + Tools (Pipeline)
*   **Architecture:** Chained execution of specialized, independent models (e.g., Whisper for audio, Phi-3 for reasoning, Stable Diffusion for generation).
*   **Mechanism:** The LLM acts as a central reasoning engine and orchestrator, processing intermediate text representations from perception tools and generating prompts for generation tools.

### Path 2: LLM + Adapters
*   **Architecture:** A frozen pre-trained LLM augmented with trainable projection layers (adapters) to align separate modalities.
*   **Mechanism:** Visual or audio encoders map inputs to the LLM's embedding space via a trained projection layer, allowing the LLM to "perceive" non-text modalities without updating its core weights.

### Path 3: Unified Multimodal Model
*   **Architecture:** A single, end-to-end trained model designed natively to process multiple modalities.
*   **Mechanism:** The model's architecture inherently supports multimodal tokens, allowing for seamless interleaving of text and image inputs during both training and inference.

## Key Observations

*   **Implementation Complexity:** Path 1 is the simplest to implement, relying on off-the-shelf APIs. Path 2 requires careful architectural surgery and training loops. Path 3 offers simplicity in usage but high complexity in initial training/fine-tuning.
*   **Data Requirements:** Path 1 requires no training data. Path 2 is highly sensitive to data quality and volume; small datasets often lead to poor alignment or hallucination. Path 3 typically requires massive pre-training datasets.
*   **Multimodal Understanding:** Path 3 generally exhibits the deepest semantic understanding of images. Path 1 is limited by the quality of the intermediate caption/transcription. Path 2 struggles with fine-grained details when trained on limited data.
*   **Limitation:** The adapter-based approach (Path 2) demonstrated significant instability and poor generalization when trained on small, domain-specific datasets, often failing to ground visual features effectively in the language space.

## Conclusion
The choice of architecture dictates the system's flexibility and resource requirements. Pipeline approaches offer modularity and immediate utility without training but suffer from information loss between stages. Adapter-based methods provide a middle ground for customization but are fragile without substantial alignment data. Unified models represent the most robust solution for deep multimodal reasoning, though they demand significant computational resources for training.
