The algorithm would review two items at a time, rearrange those not already in ascending order from left to right, and then continue to cycle through the entire sequence until it completed a pass without switching any numbers.
# Implementation

```C++
for (int i = 0; i < N; i++){
    for (int j = 0; j < N - 1; j++){
        if (conjunto[j] > conjunto[j + 1]){
            tmp = conjunto[j];
            conjunto[j] = conjunto[j + 1];
            conjunto[j + 1] = tmp;
        }
    }
}
```

# Complexity

As we are looping through the array N times for each position and there are N elements, we have a complexity $O(n^2)$. Very bad if we have a large dataset.
