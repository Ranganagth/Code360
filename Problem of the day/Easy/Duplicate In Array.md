### Problem Summary

You're given an array `arr` of size `N` that contains integers from `1` to `N-1`. Exactly one number is **duplicated**, and all others appear **at least once**.  
Your task: **Find the duplicate element**.

Example:

```
arr = [4, 2, 1, 3, 1]  
Output: 1
```

---

# Intuition Behind Floyd’s Cycle Detection

Even though this is an array, we can **visualize each index as a node**, and each value as a **pointer to another node**:

```
arr[i] → arr[arr[i]] → arr[arr[arr[i]]] → ...
```

Because one number appears **twice**, some pointer will repeat — forming a **cycle**.  
Hence, the problem reduces to **finding the starting point of a cycle in a linked list**, which is exactly what **Floyd’s Cycle Detection Algorithm** solves efficiently.

---

# Approach

1. **Phase 1: Detect a cycle**
    - Use two pointers: `slow` moves one step, `fast` moves two steps.
    - Find the intersection point inside the cycle.
        
2. **Phase 2: Find the cycle start (duplicate number)**
    - Reset `fast` to the beginning (`arr[0]`), move both `slow` and `fast` one step at a time.
    - When they meet again, that’s the **duplicate value**.

---
# Complexity Analysis

|Aspect|Value|
|---|---|
|Time Complexity|`O(N)`|
|Space Complexity|`O(1)`|
|Why?|Only two pointers used, one pass to detect cycle, another to locate entry point|

---
# Code

```java
import java.util.ArrayList;

public class Solution {

    public static int findDuplicate(ArrayList<Integer> arr) {
        // Phase 1: Use Floyd's cycle detection algorithm
        int slow = arr.get(0);
        int fast = arr.get(0);

        // Move slow by one step, fast by two steps until they meet
        do {
            slow = arr.get(slow);
            fast = arr.get(arr.get(fast));
        } while (slow != fast);

        // Phase 2: Find the start of the cycle (duplicate number)
        fast = arr.get(0);
        while (slow != fast) {
            slow = arr.get(slow);
            fast = arr.get(fast);
        }

        return slow;
    }
}
```

---

### **Example Walkthrough**

**Input:**  
`arr = [4, 2, 1, 3, 1]`

Let's trace how pointers move:

- **Initialization:**  
    `slow = 4`, `fast = 4`
    
- **First loop (detect cycle):**
    
    ```
    slow = arr[4] = 1  
    fast = arr[arr[4]] = arr[1] = 2
    
    slow = arr[1] = 2  
    fast = arr[arr[2]] = arr[1] = 2
    (slow == fast) => intersection found at 2
    ```
    
- **Second loop (find duplicate):**
    
    ```
    fast = arr[0] = 4  
    slow = 2
    
    slow = arr[2] = 1  
    fast = arr[4] = 1  
    (slow == fast) => Duplicate = 1
    ```
    

**Output: 1**

---

### **Summary**

|Step|Description|
|---|---|
|Intuition|Cycle forms due to repeated element|
|Algorithm|Floyd’s Tortoise & Hare|
|Efficiency|`O(N)` time, `O(1)` space|
|Use-case|One duplicate exists, all others ≤ N - 1|
