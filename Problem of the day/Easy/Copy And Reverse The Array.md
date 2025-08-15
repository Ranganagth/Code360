# Intuition

The problem is straightforward: we need to create a reversed copy of the given array. The simplest way is to iterate over the array from the end to the start and store the elements in a new array from start to end.

---

# Approach

1. **Create a new array** `copyArr` of the same length as `arr`.
2. Use a loop that starts from the last index of `arr` (`n-1`) and ends at the first index (`0`).
3. For each element at index `i` in the original array, place it in position `(n - 1 - i)` in the new array.
4. Return the reversed array.

This way, the first element of the original array becomes the last in the reversed array, the second becomes second-last, and so on.

---

# Complexity

* **Time complexity:**
  $O(N)$ — We traverse the array once.

* **Space complexity:**
  $O(N)$ — We store the reversed elements in a new array.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int[] copyAndReverse(int[] arr, int n) {
        int[] copyArr = new int[n];
        for (int i = 0; i < n; i++) {
            copyArr[i] = arr[n - 1 - i];
        }
        return copyArr;
    }
}
```

---

