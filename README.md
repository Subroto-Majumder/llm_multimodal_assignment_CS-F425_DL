# Multimodal AI Architectures Assignment

## Overview
This assignment explores and compares three distinct architectural approaches for building Multimodal AI systems. The goal is to understand the structural trade-offs between pipelined systems, adapter-based integration, and unified native models.

## Architectural Comparison

| Feature | Approach 1: Pipeline | Approach 2: Adapters | Approach 3: Unified |
| :--- | :--- | :--- | :--- |
| **Core Concept** | Chain of specialized tools | Frozen models + Trainable connector | Single native multimodal model |
| **Modality Bridge** | Text (Transcription/Caption) | Learned Projection Layer (MLP) | Native Token Embeddings |
| **Training Required** | **None** (Inference only) | **Low** (Train adapter only) | **High** (Full pre-training) |
| **Data Dependency** | None | High (Requires aligned pairs) | Massive (Pre-training scale) |
| **Info Loss** | High (Lossy text conversion) | Medium (Embedding alignment) | Low (End-to-end dense features) |
| **Flexibility** | High (Swap components easily) | Medium (Tied to specific encoders) | Low (Monolithic architecture) |

## Implementation Details

### Path 1: LLM + Tools (Audio Pipeline)
*   **Input:** Audio
*   **Perception:** `openai/whisper-base` (Speech-to-Text)
*   **Reasoning:** `microsoft/Phi-3-mini-4k-instruct` (4-bit quantized)
*   **Generation:** `runwayml/stable-diffusion-v1-5`
*   **Mechanism:** Audio is transcribed to text; LLM reasons on text and prompts the image generator.

### Path 2: LLM + Adapters (Visual Projection)
*   **Input:** Image
*   **Perception:** `openai/clip-vit-base-patch32` (Frozen Vision Encoder)
*   **Adapter:** Custom 2-layer MLP (Trainable)
*   **Reasoning:** `microsoft/Phi-3-mini-4k-instruct` (Frozen LLM)
*   **Generation:** `runwayml/stable-diffusion-v1-5`
*   **Mechanism:** Visual features are projected into the LLM's embedding space. Only the adapter is trained to align visual tokens with text tokens.

### Path 3: Unified Multimodal Model (Native)
*   **Input:** Image
*   **Unified Model:** `Qwen/Qwen2-VL-2B-Instruct`
*   **Generation:** `runwayml/stable-diffusion-v1-5` (External tool)
*   **Mechanism:** A single model natively processes interleaved image and text tokens to generate text responses and image prompts.

## Key Observations

*   **Pipeline (Path 1):** Most robust and easiest to implement. However, it suffers from "information bottlenecks"—if Whisper mishears a word, the LLM has no way to recover the original audio context.
*   **Adapters (Path 2):** Theoretically elegant but practically difficult. Training a projection layer on small datasets often leads to poor alignment, where the LLM fails to "ground" the visual embeddings effectively (e.g., hallucinating objects).
*   **Unified (Path 3):** Best semantic understanding. The model can reason about fine-grained visual details (colors, spatial relations) that are often lost in pipeline captions or poorly aligned adapters.

## Conclusion
The choice of architecture dictates the system's flexibility and resource requirements. **Pipelines** offer immediate utility and modularity without training. **Adapters** provide a lightweight way to customize models but are fragile without substantial data. **Unified Models** represent the state-of-the-art for deep reasoning but are computationally intensive to train from scratch.
