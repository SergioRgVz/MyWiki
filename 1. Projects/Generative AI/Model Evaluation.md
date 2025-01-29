# Language Model Evaluation Metrics

Resources of interest:
- [**HELM - Holistic Evaluation of Language Models**](https://crfm.stanford.edu/helm/latest/) - HELM is a living benchmark to evaluate Language Models more transparently.
    
- [**General Language Understanding Evaluation (GLUE) benchmark**](https://openreview.net/pdf?id=rJ4km2R5t7) - This paper introduces GLUE, a benchmark for evaluating models on diverse natural language understanding (NLU) tasks and emphasizing the importance of improved general NLU systems.
    
- [**SuperGLUE**](https://super.gluebenchmark.com/) - This paper introduces SuperGLUE, a benchmark designed to evaluate the performance of various NLP models on a range of challenging language understanding tasks.
    
- [**ROUGE: A Package for Automatic Evaluation of Summaries**](https://aclanthology.org/W04-1013.pdf) - This paper introduces and evaluates four different measures (ROUGE-N, ROUGE-L, ROUGE-W, and ROUGE-S) in the ROUGE summarization evaluation package, which assess the quality of summaries by comparing them to ideal human-generated summaries.
    
- [**Measuring Massive Multitask Language Understanding (MMLU)**](https://arxiv.org/pdf/2009.03300.pdf) - This paper presents a new test to measure multitask accuracy in text models, highlighting the need for substantial improvements in achieving expert-level accuracy and addressing lopsided performance and low accuracy on socially important subjects.
    
- [**BigBench-Hard - Beyond the Imitation Game: Quantifying and Extrapolating the Capabilities of Language Models**](https://arxiv.org/pdf/2206.04615.pdf) - The paper introduces BIG-bench, a benchmark for evaluating language models on challenging tasks, providing insights on scale, calibration, and social bias.

## Challenge of LLM Evaluation
- Traditional ML metrics like accuracy aren't suitable due to:
  - Non-deterministic outputs
  - Language-based evaluation complexity
  - Semantic similarity challenges (e.g., "Mike really loves drinking tea" vs "Mike adores sipping tea")
  - Small textual changes can mean big semantic differences (e.g., "Mike does drink coffee" vs "Mike does not drink coffee")

## Key Terminology
- Unigram: Single word
- Bigram: Two consecutive words
- N-gram: Group of n consecutive words

We have two methods:
- ## [[ROUGE (Recall Oriented Understudy for Gisting Evaluation)]]
- ## [[BLEU (Bilingual Evaluation Understudy]]
## Best Practices for Evaluation
- Use ROUGE for summarization tasks
- Use BLEU for translation tasks
- Don't rely solely on these metrics for final evaluation
- Consider using established evaluation benchmarks for comprehensive assessment
- Choose metrics based on specific use case requirements

## Implementation Notes
- Many libraries provide built-in implementations
- Consider multiple metrics for robust evaluation
- Use metrics as diagnostic tools during iteration
- Complement automated metrics with human evaluation when possible

# LLM Evaluation Benchmarks

## Why Complex Benchmarks Matter

- Simple metrics (ROUGE, BLEU) are insufficient for holistic evaluation
- Need comprehensive assessment of model capabilities
- Important to test on unseen data
- Helps understand true model capabilities across different skills

## Major Benchmark Frameworks

### GLUE (General Language Understanding Evaluation)

- Introduced: 2018
- Purpose: Encourage multi-task generalization
- Features:
    - Collection of natural language tasks
    - Sentiment analysis
    - Question-answering
    - Public leaderboard for model comparison

### SuperGLUE

- Introduced: 2019
- Improvements over GLUE:
    - More challenging versions of existing tasks
    - Additional task types
    - Multi-sentence reasoning
    - Reading comprehension
- Notable: Some models reaching human-level performance on specific tasks
    - Important distinction: Task-specific vs general human-level ability

### MMLU (Massive Multitask Language Understanding)

- Designed specifically for modern LLMs
- Tests extensive world knowledge and problem-solving
- Subject areas include:
    - Elementary mathematics
    - US history
    - Computer science
    - Law
    - Beyond basic language understanding

### BIG-bench

- Scope: 204 tasks
- Diverse testing areas:
    - Linguistics
    - Childhood development
    - Mathematics
    - Common sense reasoning
    - Biology
    - Physics
    - Social bias
    - Software development
- Available in three sizes
    - Helps manage inference costs
    - Allows for scalable evaluation

### HELM (Holistic Evaluation of Language Models)

- Focus: Improving model transparency
- Structure:
    - Seven metrics
    - 16 core scenarios
    - Multimetric approach
- Key features:
    - Beyond accuracy metrics
    - Includes fairness assessment
    - Evaluates bias
    - Measures toxicity
- Living benchmark:
    - Continuously evolving
    - New scenarios added
    - Metrics updated
    - Models added regularly

## Best Practices for Benchmark Selection

- Choose benchmarks that:
    - Isolate specific model skills
    - Focus on relevant risks
    - Test on unseen data
    - Match your use case
- Consider multiple aspects:
    - Task-specific performance
    - General capabilities
    - Potential risks and biases
- Review benchmark leaderboards for comparison
- Consider cost implications of comprehensive testing