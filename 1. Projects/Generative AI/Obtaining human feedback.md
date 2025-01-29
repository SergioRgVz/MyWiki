# RLHF (Reinforcement Learning with Human Feedback) Fine-tuning Process

## 1. Initial Setup

- Select a base model with basic capabilities in your target task
- Recommended: Start with an instruct model that has general capabilities
- Generate multiple responses for each prompt in your dataset

## 2. Human Feedback Collection

- Choose specific criteria for evaluation (e.g., helpfulness, toxicity)
- Have human labelers assess and rank completions
- Key practices:
    - Use multiple labelers per prompt to establish consensus
    - Provide detailed instructions to ensure quality feedback
    - Allow labelers to use internet for fact-checking
    - Include options for handling ties and nonsensical answers

## 3. Data Preparation for Reward Model

- Convert ranking data into pairwise comparisons
- For N completions, create N choose 2 pairs
- Scoring system:
    - Preferred response gets 1
    - Less preferred response gets 0
- Reorder prompts to put preferred completion first (Yj)

## Important Notes

- Ranked feedback provides more training data than binary (thumbs up/down) feedback
- Clear labeler instructions are crucial for consistent, quality feedback
- Diverse, global thinking should be represented in the labeler pool
- Poor quality answers can be flagged instead of ranked for easy removal

This process creates the foundation for training the reward model, which will eventually replace human evaluators in the reinforcement learning fine-tuning process.