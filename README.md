# Text-to-SVG Generation with Fine-Tuned LLMs  
Fine-tuning LLaMA and Mistral Models to Generate SVG Code from Natural Language  
Based on our final project report for CSE 575 at Arizona State University. :contentReference[oaicite:0]{index=0}

---

## Overview
This repository contains our implementation for generating Scalable Vector Graphics (SVG) directly from natural-language prompts using fine-tuned large language models (LLMs).  
Unlike diffusion-based vector synthesis methods, we treat SVG as **text-to-code generation**, enabling LLMs to output valid SVG markup directly.

We fine-tuned several open-source models—including **LLaMA (8B, 12B)** and **Mistral 7B**—using parameter-efficient fine-tuning (LoRA and QLoRA), allowing training on modest academic hardware.

Our experiments show that a fine-tuned **Mistral 7B** produces syntactically valid and semantically aligned SVGs, competitive with diffusion-based methods while being significantly faster.

---

## Key Contributions
- **Pure LLM-based SVG generation**  
  Generate SVG code directly from text prompts without diffusion models or vision encoders.

- **Efficient fine-tuning with LoRA/QLoRA**  
  Only small trainable modules added; base model frozen, enabling training on limited GPU memory.

- **Comprehensive evaluation**  
  Training loss comparison, CLIP-based semantic scoring, and qualitative SVG assessment.

---

## Dataset
We use the **text2svg-stack** dataset (StarVector), consisting of:

- ~2.17M text–SVG pairs  
- Captions from BLIP-2, CogVLM, and LLaVA  
- SVGs including icons, logos, illustrations, and simple scenes  

For training stability and hardware constraints, we filtered by SVG string length and trained on subsets of **50k, 100k, 200k, and 300k** samples.

Each sample contains:
- `filename`
- `svg` (raw markup)
- `caption_blip2` (chosen as input)
- `caption_cogvlm`
- `caption_llava`

---

## Model Architecture & Training Pipeline

### Models Fine-Tuned
- LLaMA 8B  
- LLaMA 12B  
- Mistral 7B  
- Additional experiments: T5, Gemma, Qwen Coder  

### Training Setup
- LoRA or QLoRA adapters  
- 4-bit quantization via BitsAndBytes  
- Gradient checkpointing enabled  
- Sequence length fixed at 256  
- Chat-style prompt formatting:

## Training Parameters
- Epochs: 5 (baseline setting)
- Learning rate: 2e-4
- Batch size: 292
- Checkpoint frequency: every 100 steps
- Gradient accumulation: disabled
- LoRA target modules: `q_proj`, `k_proj`, `v_proj`, `o_proj`, `gate_proj`

---

## Results

### Training Loss Comparison
Across all tested models, Mistral 7B achieved the lowest training loss:

| Model | Parameter Size | Training Loss |
|-------|----------------|----------------|
| T5 Base | 220M | ~2.062 |
| T5 Small | 60M | ~0.970 |
| Mistral 7B | 7B | ~0.616 |
| Gemma3 1B | 1B | ~0.784 |
| Gemma3 4B | 4B | ~1.027 |
| Qwen2.5 Coder 3B | 3B | ~0.814 |
| Qwen2.5 Coder 7B | 7B | ~1.456 |
| Qwen2.5 Coder 14B | 14B | ~3.957 |

---

### Mistral 7B Parameter Experiments
We further experimented with dataset sizes, epochs, and batch sizes:

| Dataset Size | Epochs | Batch Size | Training Loss |
|--------------|--------|------------|----------------|
| 50,000 | 10 | 290 | 0.5734 |
| 100,000 | 14 | 100 | 0.4775 |
| 200,000 | 5 | 100 | 0.6498 |
| 300,000 | 3 | 292 | 0.9872 |

---

## CLIP Evaluation
We evaluated the fine-tuned Mistral 7B model on 100 unseen prompts.

- Valid SVG outputs: 73  
- Successfully rasterized SVGs: 71  
- Average CLIP score: **27.4112%**

### Quantitative Comparison

| Method / Model | CLIP Score | Time per Prompt |
|----------------|------------|------------------|
| SVGDreamer | 0.360 | 35 min 12 sec |
| DiffSketch | 0.310 | 10 min 22 sec |
| CLIPDraw | 0.249 | 5 min 10 sec |
| **Mistral 7B (Fine-tuned)** | **0.274** | **9 seconds** |

The fine-tuned Mistral model provides a strong trade-off between quality and generation speed.

---

## Qualitative Results
The fine-tuned model consistently generated:

- Valid and compact SVG XML
- Coherent geometric structures
- Semantically aligned shapes, especially for black-and-white icons
- Better boundary preservation than baseline LLMs (GPT-4o, Gemini)

Examples include:
- Circular arrow icons  
- Chinese “love” character in stylized font  
- Speech bubble icons  

---

## Conclusion
Our experiments demonstrate the following:

1. **Model capacity is important, but quality matters equally.**  
   Mistral 7B performed better than larger models under identical training setups.

2. **LLM-based SVG generation is extremely fast.**  
   SVGs are generated in seconds, compared to minutes for diffusion-based methods.

3. **Parameter-efficient fine-tuning (LoRA/QLoRA) is essential.**  
   It enables training large models on academic hardware without loss in fidelity.

---

## Future Work
- Incorporating iterative refinement using CLIP feedback  
- Exploring code-focused LLMs (Code Llama, StarCoder)  
- Multi-stage generation pipeline similar to Chat2SVG  
- Automatic SVG syntax validation and error correction  

---

## Citation
If you use this repository, please cite our project report:

*CSE 575 Final Project Report — SVG Generation with Fine-Tuned LLMs*  
:contentReference[oaicite:0]{index=0}


