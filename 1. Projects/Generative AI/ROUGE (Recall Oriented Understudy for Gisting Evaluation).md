Primary use: Evaluating automatically generated summaries

### ROUGE-1 (Unigram-based)
- Metrics calculated:
  - Recall: $Matched\ unigrams /\ Reference\ unigrams$
  - Precision: $Matched\ words /\ Output\ words$
  - F1 score: Harmonic mean of recall and precision
![[Pasted image 20250108125647.png]]
- Limitations:
  - Doesn't consider word ordering
  - Can give high scores to poor quality outputs
  - Negations can be missed
![[Pasted image 20250108125742.png]]

### ROUGE-2 (Bigram-based)
- Uses pairs of words for matching
- Generally produces lower scores than ROUGE-1
- Better at capturing word order
- Scores decrease with longer sentences
![[Pasted image 20250108125800.png]]

### ROUGE-L (Longest Common Subsequence)
- Based on longest matching word sequences
- Metrics use LCS length for calculations
- More sophisticated than simple n-gram matching
![[Pasted image 20250108125920.png]]

### ROUGE Implementation Notes
- Scores only comparable within same task type
- Clipping function can help prevent gaming through word repetition
![[Pasted image 20250108130053.png]]
- Available in libraries like Hugging Face
- N-gram size selection depends on:
  - Sentence length
  - Use case requirements
  - Specific evaluation needs