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

