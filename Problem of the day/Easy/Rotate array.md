# Intuition
To rotate an array `k` times to the left, each element shifts `k` places to the left, and the first `k` elements wrap around to the end. Instead of rotating step by step, we can directly compute the result using array slicing.

# Approach
1. Normalize `k` by taking `k % n` to avoid unnecessary full rotations.
2. Slice the array into two parts:
   - First part: from index `k` to the end
   - Second part: from index `0` to `k - 1`
3. Append the two parts together to form the rotated array.
4. Return the new array.

This avoids multiple shifts and gives a time complexity of O(n).

# Complexity
- **Time complexity:** O(n), where `n` is the number of elements in the array, as we visit each element once.
- **Space complexity:** O(n), since we're creating a new array list for the result.

# Code
```java
import java.util.ArrayList;

public class Solution {
    public static ArrayList<Integer> rotateArray(ArrayList<Integer> arr, int k) {
        int n = arr.size();
        k = k % n; // Normalize the rotation
        ArrayList<Integer> result = new ArrayList<>();

        // Add elements from index k to n-1
        for (int i = k; i < n; i++) {
            result.add(arr.get(i));
        }

        // Add first k elements
        for (int i = 0; i < k; i++) {
            result.add(arr.get(i));
        }

        return result;
    }
}
```
