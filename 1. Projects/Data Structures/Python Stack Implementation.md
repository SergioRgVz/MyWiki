Here is the implementation of a stack in Python.

```python
# Stack implementation in python

# Creating a stack
def create_stack():
    stack = []
    return stack

# Check if the stack is empty
def is_empty(stack):
    return len(stack) == 0

# Add an element to the stack
def push(stack, item):
    stack.append(item)
    print("pushed to stack " + item)

# Remove an element from the stack
def pop(stack):
    if is_empty(stack):
        return "stack is empty"
    return stack.pop()

stack = create_stack()
push(stack, str(1))
push(stack, str(2))
push(stack, str(3))
print("popped from stack " + pop(stack))
print("stack after popping an element: " + str(stack))

# Output:
# pushed to stack 1
# pushed to stack 2
# pushed to stack 3
# popped from stack 3
# stack after popping an element: ['1', '2']
```