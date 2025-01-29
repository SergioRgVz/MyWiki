## Overview

- Type: Reparameterization PEFT method
- Core concept: Uses rank decomposition matrices alongside frozen original weights
- Primary application: Self-attention layers in transformer models

Resources:
- [**LoRA Low-Rank Adaptation of Large Language Models**](https://arxiv.org/pdf/2106.09685.pdf) - This paper proposes a parameter-efficient fine-tuning method that makes use of low-rank decomposition matrices to reduce the number of trainable parameters needed for fine-tuning language models.
    
- [**QLoRA: Efficient Finetuning of Quantized LLMs**](https://arxiv.org/pdf/2305.14314.pdf) - This paper introduces an efficient method for fine-tuning large language models on a single GPU, based on quantization, achieving impressive results on benchmark tests.
## How LoRA Works

### Technical Process

1. Freezes all original model parameters
2. Injects pair of rank decomposition matrices
3. Trains only the smaller matrices
4. During inference:
    - Multiplies low-rank matrices together
    - Adds result to original frozen weights
    - Replaces original weights with updated values

![[Pasted image 20250108152423.png]]

### Implementation Details

- Primarily applied to self-attention layers because:
    - Contains most of the model's parameters
    - Provides biggest savings in trainable parameters
- Can be applied to other components (e.g., feed-forward layers)
- No significant impact on inference latency

## Practical Example

Using transformer architecture from "Attention is All You Need":

### Original Model

- Weight matrix dimensions: 512 x 64
- Total parameters: 32,768

### With LoRA (rank = 8)

- Matrix A: 8 x 64 (512 parameters)
- Matrix B: 512 x 8 (4,096 parameters)
- Total trainable parameters: 4,608
- Parameter reduction: 86%

## Multi-Task Capabilities

- Can train different LoRA matrices for different tasks
- Easy weight switching at inference time:
    1. Multiply task-specific LoRA matrices
    2. Add to original frozen weights
    3. Update model with new weights
- Minimal memory requirements for storing multiple task matrices
![[Pasted image 20250108152500.png]]
## Performance Analysis

### ROUGE Score Comparison (Dialogue Summarization)

1. Base FLAN-T5: Baseline performance
2. Full Fine-tuning: +0.19 ROUGE-1 improvement
3. LoRA Fine-tuning: +0.17 ROUGE-1 improvement

- Nearly matches full fine-tuning performance
- Requires significantly less compute

## Choosing LoRA Rank

### Research Findings

- Optimal rank range: 4-32
- Performance plateau observed at rank > 16
- Considerations:
    - Lower rank = fewer trainable parameters
    - Higher rank ≠ necessarily better performance

### Trade-offs

- Computing requirements vs. model performance
- Storage efficiency vs. task adaptability
- Training speed vs. accuracy

## Benefits

1. Reduced computational requirements:
    - Can run on single GPU
    - No need for distributed computing
2. Storage efficiency:
    - Small matrix sizes
    - Easy to store multiple task-specific adaptations
3. Performance:
    - Near full fine-tuning quality
    - Minimal inference latency impact
4. Flexibility:
    - Easy task switching
    - Maintainable for multiple tasks

## Current State

- Active area of research
- Best practices still evolving
- Proven effective across various applications
- Applicable beyond LLMs to other domains