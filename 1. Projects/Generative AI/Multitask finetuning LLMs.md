# Multitask Fine-tuning and FLAN Models

## Multitask Fine-tuning Overview
- Extension of single task fine-tuning where training dataset includes multiple tasks
- Training dataset contains examples for various tasks:
  - Summarization
  - Review rating
  - Code translation
  - Entity recognition
- Model improves performance on all tasks simultaneously
- Avoids catastrophic forgetting through mixed dataset training
- Weights are updated over many epochs using calculated losses

### Requirements and Benefits
- Requires substantial data (50-100k examples in training set)
- Results in highly capable models
- Suitable for situations requiring good performance across multiple tasks

## FLAN Models
FLAN (Fine-tuned Language Net) is a specific instruction set for fine-tuning different models.

### Key Characteristics
- Applied as final training step ("dessert" after pre-training "main course")
- Creates different versions:
  - FLAN-T5 (based on T5 foundation model)
  - FLAN-PALM (based on PALM foundation model)

### FLAN-T5 Specifics
- General purpose instruct model
- Fine-tuned on 473 datasets
- Covers 146 task categories

### Example Dataset: SAMSum
- Part of muffin collection
- Purpose: Training models for dialogue summarization
- Contains 16,000 messenger-like conversations with summaries
- Created by linguists to reflect real-life messenger conversations
- Includes crafted summaries with important information and names

### Prompt Template Features
- Uses multiple instructions for same task
- Helps model generalization
- Examples:
  - "Briefly summarize that dialogue"
  - "What is a summary of this dialogue?"
  - "What was going on in that conversation?"

### Additional Fine-tuning Example: DialogSum
- Contains 13,000+ support chat dialogues and summaries
- Not included in FLAN-T5 training data
- Used to improve specific dialogue summarization capabilities
- Demonstrates benefits of domain-specific fine-tuning

## Benefits of Custom Fine-tuning
- Can improve model performance for specific use cases
- Best results achieved using company's internal data
- Helps model learn company-specific summarization preferences
- Can address gaps in pre-trained model capabilities

## Important Considerations
- Evaluation metrics needed to assess model quality
- Compare fine-tuned version performance with base model
- Consider specific use case requirements
- May need additional domain-specific training data

Another way of fine-tuning is using [[Parameter Efficient Fine-Tuning (PEFT)]] approach.

