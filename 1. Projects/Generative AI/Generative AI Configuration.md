Inside the Generative AI models, there are usually 4 parameters we can configure:
- Num Max tokens: this declares the number of maximum tokens that the model can generate when we do an inference
- Top K: this select an output from the top-k parameters that we chose, for example, if we set top-k to 4 the model will only select one random token from the 4 first tokens (in terms of probability order)
- Top P: the top P, is set to select an output using the random-weighted strategy with the top-ranked consecutive results by probability and with a cumulative probability <= p
- Temperature: the temperature is set to uniform the probabilities of the output tokens in order to set more randomnes to the model output or not, the higher the temperature the higher the creativity of the model.