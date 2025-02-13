In stack, elements are stored in the LIFO principle. That is, the last element stored in a stack will be removed first. 

It works just like a pile of plates where the last plate kept on the pile will be removed first.

![[Pasted image 20241006180739.png]]

# Operations
Stack has these operations:
- **Push**: add an element to the top of a stack
- **Pop**: remove an element from the top of a stack
- **IsEmpty**: check if the stack is empty
- **IsFull**: check if the stack is full
- **Peek**: get the value of the top element without removing it

# Working of Stack

The operations work as follows:
1. A pointer called `TOP` is used to keep track of the top element in the stack.
2. When initializing the stack, we set its value to -1 so that we can check if the stack is empty by comparing `TOP == -1`.
3. On pushing an element, we increase the value of `TOP` and place the new element in the position pointed by `TOP`.
4. On popping an element, we return the element pointed to by `TOP` and reduce its value.
5. Before pushing, we check if the stack is already full.
6. Before popping, we check if the stack is already empty.

![[Pasted image 20241006181705.png]]

# Implementation

We have these examples in some programming languages:
- [[Python Stack Implementation]]
- [[C Stack Implementation]]
- [[C++ Stack Implementation]]
- [[Java Stack Implementation]]

# Time complexity
For the array based implementation of a stack, the push and pop operations take constant time `O(1)`.

# Applications
- To reverse a word
- In compilers, compilers use the stack to calculate the value of expressions like `2 + 4 / 5 * (7 - 9)` by converting the expression to prefix or postfix form.
- In browsers, the back button in a browser saves all the URLs you have visisted previously in a stack.