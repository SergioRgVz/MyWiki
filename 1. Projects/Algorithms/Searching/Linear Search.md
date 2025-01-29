In Linear Search, we iterate over all the elements of the array and check if it the current element is equal to the target element. If we find any element to be equal to the target element, then return the index of the current element. Otherwise, if no element is equal to the target element, then return -1 as the element is not found. Linear search is also known as sequential search.
# Implementation
```python
def search(arr, N, x):

    for i in range(0, N):
        if (arr[i] == x):
            return i
    return -1


# Driver Code
if __name__ == "__main__":
    arr = [2, 3, 4, 10, 40]
    x = 10
    N = len(arr)

    # Function call
    result = search(arr, N, x)
    if(result == -1):
        print("Element is not present in array")
    else:
        print("Element is present at index", result)
```

# Time and Space Complexity of Linear Search Algorithm:

***Time Complexity:***

- ***Best Case:*** In the best case, the key might be present at the first index. So the best case complexity is O(1)
- ***Worst Case:** In the worst case, the key might be present at the last index i.e., opposite to the end from which the search has started in the list. So the worst-case complexity is O(N) where N is the size of the list.
- ***Average Case:*** O(N)

***Auxiliary Space:** O(1) as except for the variable to iterate through the list, no other variable is used.

## ***Advantages of Linear Search*** Algorithm:

- Linear search can be used irrespective of whether the array is sorted or not. It can be used on arrays of any data type.
- Does not require any additional memory.
- It is a well-suited algorithm for small datasets.

## ***Disadvantages of Linear Search*** Algorithm:****

- Linear search has a time complexity of O(N), which in turn makes it slow for large datasets.
- Not suitable for large arrays.