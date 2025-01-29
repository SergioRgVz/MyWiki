Resources:
- [**Scaling Down to Scale Up: A Guide to Parameter-Efficient Fine-Tuning**](https://arxiv.org/pdf/2303.15647.pdf) - This paper provides a systematic overview of Parameter-Efficient Fine-tuning (PEFT) Methods in all three categories discussed in the lecture videos.
    
- [**On the Effectiveness of Parameter-Efficient Fine-Tuning**](https://arxiv.org/pdf/2211.15583.pdf) - The paper analyzes sparse fine-tuning methods for pre-trained models in NLP.

## Challenge with Full Fine-tuning
- Requires extensive computational resources:
  - Memory for model weights (hundreds of gigabytes)
  - Additional memory for:
    - Optimizer states
    - Gradients
    - Forward activations
    - Temporary memory
  - Multiple versions of full-sized models for different tasks

## PEFT Advantages
- Updates only a small subset of parameters
- Memory requirements:
  - As low as 15-20% of original LLM weights
  - Can often run on a single GPU
- Reduces catastrophic forgetting
- Storage efficient:
  - Small footprint (megabytes)
  - Easy to swap PEFT weights for different tasks
  - Original model remains unchanged

## Main PEFT Methods

### 1. Selective Methods
- Fine-tunes subset of original LLM parameters
- Approaches:
  - Training specific components
  - Training specific layers
  - Training specific parameter types
- Note: Mixed performance with efficiency trade-offs

### 2. Reparameterization Methods
- Creates low-rank transformations of original weights
- Key example: [[LoRA (Low-Rank Adaptation)]]
- Reduces number of trainable parameters
- Works with original LLM parameters

### 3. Additive Methods
- Keeps original weights frozen
- Two main approaches:

#### Adapter Methods
- Adds new trainable layers
- Typically inserted in:
  - Encoder components
  - Decoder components
  - After attention layers
  - After feed-forward layers

#### Soft Prompt Methods
- Keeps model architecture frozen
- Focuses on input manipulation
- Techniques:
  - Adding trainable parameters to prompt embeddings
  - Retraining embedding weights
- Example: [[Prompt tuning]]

![[Pasted image 20250108154512.png]]

## Considerations for Method Selection
- Parameter efficiency
- Memory efficiency
- Training speed
- Model quality
- Inference costs

## Implementation Benefits
- More manageable training requirements
- Flexible deployment
- Efficient multi-task adaptation
- Reduced storage needs
- Preservation of base model capabilities
