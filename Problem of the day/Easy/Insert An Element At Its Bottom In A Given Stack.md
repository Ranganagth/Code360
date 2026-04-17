# Intuition

Stack only allows access from the **top**, but insertion is required at the **bottom**.

Use recursion to:

* Remove all elements
* Insert `X` when stack becomes empty
* Push back removed elements

This simulates bottom insertion without extra data structures.

---

# Approach

1. Base case:

   * If stack is empty → push `X`
2. Recursive step:

   * Pop top element
   * Recursively insert `X` at bottom
   * Push popped element back

---

# Complexity

* **Time complexity:**
  $$O(N)$$

* **Space complexity:**
  $$O(N)$$ (recursion stack)

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution 
{
  public static Stack<Integer> pushAtBottom(Stack<Integer> myStack, int x) 
  {
    if (myStack.isEmpty()) {
      myStack.push(x);
      return myStack;
    }

    int top = myStack.pop();

    pushAtBottom(myStack, x);

    myStack.push(top);

    return myStack;
  }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Stack: [7, 1, 4, 5] (top = 5)

Pop sequence:

* pop 5
* pop 4
* pop 1
* pop 7

Stack becomes empty → push 9

Now rebuild:

* push 7
* push 1
* push 4
* push 5

Final:
[9, 7, 1, 4, 5]

---

## Example 2:

Stack: [4]

Pop 4 → empty → push 0 → push 4

Result:
[0, 4]
