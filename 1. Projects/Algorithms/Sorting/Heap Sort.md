Heap sort is a comparison-based sorting technique based on Binary Heap Data Structure. It can be seen as an optimization over selection sort where we first find the max (or min) element and swap it with the last (or first). We repeat the same process for the remaining elements. In Heap Sort, we use Binary Heap so that we can quickly find and move the max element in $O(Log\ n)$ instead of $O(n)$ and hence achieve the $O(n\ Log \ n)$ time complexity.
# Algorithm

First convert the array into a [max heap](https://www.geeksforgeeks.org/introduction-to-max-heap-data-structure/) using ***heapify****, Please note that this happens in-place. The array elements are re-arranged to follow heap properties. Then one by one delete the root node of the Max-heap and replace it with the last node and ***heapify****. Repeat this process while size of heap is greater than 1.

- Rearrange array elements so that they form a Max Heap.
- Repeat the following steps until the heap contains only one element:
    - Swap the root element of the heap (which is the largest element in current heap) with the last element of the heap.
    - Remove the last element of the heap (which is now in the correct position). We mainly reduce heap size and do not remove element from the actual array.
    - Heapify the remaining elements of the heap.
- Finally we get sorted array.
# Implementation
```python
# To heapify a subtree rooted with node i
# which is an index in arr[].
def heapify(arr, n, i):
    
     # Initialize largest as root
    largest = i 
    
    #  left index = 2*i + 1
    l = 2 * i + 1 
    
    # right index = 2*i + 2
    r = 2 * i + 2  

    # If left child is larger than root
    if l < n and arr[l] > arr[largest]:
        largest = l

    # If right child is larger than largest so far
    if r < n and arr[r] > arr[largest]:
        largest = r

    # If largest is not root
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]  # Swap

        # Recursively heapify the affected sub-tree
        heapify(arr, n, largest)

# Main function to do heap sort
def heapSort(arr):
    
    n = len(arr) 

    # Build heap (rearrange array)
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # One by one extract an element from heap
    for i in range(n - 1, 0, -1):
      
        # Move root to end
        arr[0], arr[i] = arr[i], arr[0] 

        # Call max heapify on the reduced heap
        heapify(arr, i, 0)

def printArray(arr):
    for i in arr:
        print(i, end=" ")
    print()

# Driver's code
arr = [9, 4, 3, 8, 10, 2, 5] 
heapSort(arr)
print("Sorted array is ")
printArray(arr)
```
# Complexity
***Time Complexity:*** $O(n\ log\ n)$  
***Auxiliary Space:*** $O(log\ n)$

# **Advantages of Heap Sort***
- ***Efficient Time Complexity:*** Heap Sort has a time complexity of O(n log n) in all cases. This makes it efficient for sorting large datasets. The ****log n**** factor comes from the height of the binary heap, and it ensures that the algorithm maintains good performance even with a large number of elements.
- ***Memory Usage:*** Memory usage can be minimal (by writing an iterative heapify() instead of a recursive one). So apart from what is necessary to hold the initial list of items to be sorted, it needs no additional memory space to work
- **Simplicity:*** It is simpler to understand than other equally efficient sorting algorithms because it does not use advanced computer science concepts such as recursion.

# Disadvantages of Heap Sort
- **Costly***: Heap sort is costly as the constants are higher compared to merge sort even if the time complexity is O(n Log n) for both.
- ***Unstable***: Heap sort is unstable. It might rearrange the relative order.
- ***Inefficient:*** Heap Sort is not very efficient because of the high constants in the time complexity.