Merge sort is a sorting algorithm that follows the divide-and-conquer approach. It works by recursively dividing the input array into smaller subarrays and sorting those subarrays then merging them back together to obtain the sorted array.

In simple terms, we can say that the process of merge sort is to divide the array into two halves, sort each half, and then merge the sorted halves back together. This process is repeated until the entire array is sorted.
![[Pasted image 20250123132015.png]]
![[Pasted image 20250123132515.png]]
# Algorithm
1. ***Divide:*** Divide the list or array recursively into two halves until it can no more be divided.
2. ***Conquer:*** Each subarray is sorted individually using the merge sort algorithm.
3. ***Merge:*** The sorted subarrays are merged back together in sorted order. The process continues until all elements from both subarrays have been merged.
# Implementation

```python
def mergeSort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    leftHalf = arr[:mid]
    rightHalf = arr[mid:]

    sortedLeft = mergeSort(leftHalf)
    sortedRight = mergeSort(rightHalf)

    return merge(sortedLeft, sortedRight)

def merge(left, right):
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])

    return result

unsortedArr = [3, 7, 6, -10, 15, 23.5, 55, -13]
sortedArr = mergeSort(unsortedArr)
```

# Complexity

- ***Time Complexity:***
    - ***Best Case:*** $O(n log n)$, When the array is already sorted or nearly sorted.
    - ***Average Case:****$O(n log n)$, When the array is randomly ordered.
    - ***Worst Case:*** $O(n log n)$, When the array is sorted in reverse order.
- ***Auxiliary Space:*** $O(n)$, Additional space is required for the temporary array used during merging.

## ***Advantages of Merge Sort:***

- ***Stability*** : Merge sort is a stable sorting algorithm, which means it maintains the relative order of equal elements in the input array.
- ***Guaranteed worst-case performance:*** Merge sort has a worst-case time complexity of ****O(N logN)**** , which means it performs well even on large datasets.
- ***Simple to implement:*** The divide-and-conquer approach is straightforward.
- ***Naturally Parallel*** : We independently merge subarrays that makes it suitable for parallel processing.

## ***Disadvantages of Merge Sort:***

- ***Space complexity:*** Merge sort requires additional memory to store the merged sub-arrays during the sorting process.
- ***Not in-place:*** Merge sort is not an in-place sorting algorithm, which means it requires additional memory to store the sorted data. This can be a disadvantage in applications where memory usage is a concern.
- ***Slower than QuickSort in general****. QuickSort is more cache friendly because it works in-place.