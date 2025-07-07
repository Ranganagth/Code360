### Problem Understanding

You are given a **sorted array `arr`** of size `n`.  
Your task is to **remove duplicates in-place**, so that each element appears **only once** and return the **length of the modified array**.

> **Constraints**:  
- Must work in **O(n)** time.  
- Use only **O(1)** extra space.  
- **Sorted array** — this is key.

---

# Intuition

Since the array is sorted, **all duplicates will be adjacent**.  
So we can just iterate through the array and **shift unique elements forward**.

---

# Approach (Two Pointers)

- Use one pointer `i` to mark the **position of last unique element**.
- Traverse the array using another pointer `j`.
- If `arr[j] != arr[i]`, then we found a **new unique element**.
    - Increment `i` and copy `arr[j]` to `arr[i]`.
- Finally, the first `i + 1` elements are the unique ones.

---
# Complexity Analysis

- **Time**: O(n) — single pass through the array
- **Space**: O(1) — in-place operation

---

# Code

```java
public class Solution {
    public static int removeDuplicates(int[] arr, int n) {
        if (n == 0) return 0;

        int i = 0; // Points to the last unique element

        for (int j = 1; j < n; j++) {
            if (arr[j] != arr[i]) {
                i++;
                arr[i] = arr[j];
            }
        }

        return i + 1; // New length
    }
}
```

---

### **Example Walkthrough**

**Input**:  
`n = 10`  
`arr = [1, 2, 2, 3, 3, 3, 4, 4, 5, 5]`

**Output**:  
`5`  
Modified array: `[1, 2, 3, 4, 5, _, _, _, _, _]`

---
