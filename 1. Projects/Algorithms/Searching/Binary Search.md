***Binary Search*** ***Algorithm*** is a searching algorithm used in a sorted array by **repeatedly dividing the search interval in half**. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to $O(log\ N)$.

# Algorithm
- Divide the search space into two halves by [***finding the middle index “mid”**](https://www.geeksforgeeks.org/problem-binary-search-implementations/). 
- Compare the middle element of the search space with the ***key****. 
- If the ***key*** is found at middle element, the process is terminated.
- If the ***key*** is not found at middle element, choose which half will be used as the next search space.
    - If the ***key*** is smaller than the middle element, then the ***left*** side is used for next search.
    - If the ***key*** is larger than the middle element, then the ***right*** side is used for next search.
- This process is continued until the ***key*** is found or the total search space is exhausted.

# Implementation
```python
# Iterative version
def binarySearch(arr, low, high, x):

    while low <= high:

        mid = low + (high - low) // 2

        # Check if x is present at mid
        if arr[mid] == x:
            return mid

        # If x is greater, ignore left half
        elif arr[mid] < x:
            low = mid + 1

        # If x is smaller, ignore right half
        else:
            high = mid - 1

    # If we reach here, then the element
    # was not present
    return -1


# Driver Code
if __name__ == '__main__':
    arr = [2, 3, 4, 10, 40]
    x = 10

    # Function call
    result = binarySearch(arr, 0, len(arr)-1, x)
    if result != -1:
        print("Element is present at index", result)
    else:
        print("Element is not present in array")

# Recursive version
def binarySearch(arr, low, high, x):

    # Check base case
    if high >= low:

        mid = low + (high - low) // 2

        # If element is present at the middle itself
        if arr[mid] == x:
            return mid

        # If element is smaller than mid, then it
        # can only be present in left subarray
        elif arr[mid] > x:
            return binarySearch(arr, low, mid-1, x)

        # Else the element can only be present
        # in right subarray
        else:
            return binarySearch(arr, mid + 1, high, x)

    # Element is not present in the array
    else:
        return -1


# Driver Code
if __name__ == '__main__':
    arr = [2, 3, 4, 10, 40]
    x = 10
    
    # Function call
    result = binarySearch(arr, 0, len(arr)-1, x)
    
    if result != -1:
        print("Element is present at index", result)
    else:
        print("Element is not present in array")
```

# Complexity
- ***Time Complexity:*** 
    - Best Case: $O(1)$
    - Average Case: $O(log\ N)$
    - Worst Case: $O(log\ N)$
- ***Auxiliary Space:*** $O(1)$, If the recursive call stack is considered then the auxiliary space will be $O(log\ N)$.

## ***Advantages of Binary Search**

- Binary search is faster than linear search, especially for large arrays.
- More efficient than other searching algorithms with a similar time complexity, such as interpolation search or exponential search.
- Binary search is well-suited for searching large datasets that are stored in external memory, such as on a hard drive or in the cloud.

## ***Disadvantages of Binary Search***

- The array should be sorted.
- Binary search requires that the data structure being searched be stored in contiguous memory locations. 
- Binary search requires that the elements of the array be comparable, meaning that they must be able to be ordered.