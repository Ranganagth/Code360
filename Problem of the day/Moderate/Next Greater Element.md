# Intuition

For each element in the array, we must find the first greater element to its **right**.
A **brute-force** approach (checking every element to the right) would take **O(N²)** — too slow for N up to 10⁵.

We can optimize this using a **monotonic stack**.

---

# Approach (Monotonic Stack)

We use a **stack** to keep track of possible "next greater" candidates while iterating **from right to left**:

1. Initialize an empty stack and an array `res` of size `n` to store results.
2. Traverse the array **from right to left** (i = n-1 → 0):

   * While the stack is **not empty** and the **top of stack ≤ arr[i]**, pop from the stack (since it cannot be the next greater for this element or any to its left).
   * If stack is **empty**, that means no greater element exists → `res[i] = -1`.
   * Else, the **top of stack** is the **Next Greater Element** → `res[i] = stack.peek()`.
   * Push `arr[i]` onto the stack.
3. Return `res`.

---

# Complexity

| Type  | Complexity                                             |
| ----- | ------------------------------------------------------ |
| Time  | **O(N)** — each element pushed and popped at most once |
| Space | **O(N)** — for the stack and result array              |

---

# Code

```java
import java.util.*;
import java.io.*; 

public class Solution {
	
	public static int[] nextGreater(int[] arr, int n) {	
		int[] res = new int[n];
		Stack<Integer> stack = new Stack<>();

		for (int i = n - 1; i >= 0; i--) {
			// Remove smaller or equal elements
			while (!stack.isEmpty() && stack.peek() <= arr[i]) {
				stack.pop();
			}

			// If stack empty -> no greater element
			if (stack.isEmpty()) {
				res[i] = -1;
			} else {
				res[i] = stack.peek();
			}

			// Push current element
			stack.push(arr[i]);
		}

		return res;
	}
};

```

---

## Example Walkthrough

**Input:** `[1, 2, 4, 3]`

| i | arr[i] | Stack (before) | NGE | Stack (after) |
| - | ------ | -------------- | --- | ------------- |
| 3 | 3      | []             | -1  | [3]           |
| 2 | 4      | [3] → pop(3)   | -1  | [4]           |
| 1 | 2      | [4]            | 4   | [4, 2]        |
| 0 | 1      | [4, 2]         | 2   | [4, 2, 1]     |

**Output:** `[2, 4, -1, -1]`

---
