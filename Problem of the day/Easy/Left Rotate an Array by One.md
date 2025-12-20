# Intuition

Left rotation by one shifts every element one index left. The first element loses its position and must reappear at the end. No reordering logic is needed beyond preserving sequence.

# Approach

1. Store the first element.
2. Shift elements from index `1` to `n-1` one position to the left.
3. Place the stored first element at index `n-1`.
4. Return the modified array.

This performs the rotation in-place with linear traversal.

# Complexity

* **Time:** `O(n)`
* **Space:** `O(1)` extra space

# Code

```java
public class Solution {

    static int[] rotateArray(int[] arr, int n) {
        if (n <= 1) return arr;

        int first = arr[0];

        for (int i = 1; i < n; i++) {
            arr[i - 1] = arr[i];
        }

        arr[n - 1] = first;
        return arr;
    }
}
```

# Example Walkthrough

**Input:**

`arr = [5, 7, 3, 2]`, `n = 4`

Step 1: Store first element
`first = 5`

Step 2: Shift left
`[7, 3, 2, 2]`

Step 3: Place first at end
`[7, 3, 2, 5]`

**Output:**
`[7, 3, 2, 5]`
