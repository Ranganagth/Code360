# Intuition

To solve the problem of making a binary string *beautiful* (i.e., alternating 0s and 1s), we need to compare the input string against **two ideal alternating patterns**:
* **Pattern 1 (starting with '0')**: `010101...`
* **Pattern 2 (starting with '1')**: `101010...`

We compute how many characters differ from each pattern and return the minimum of the two counts.

# Approach

1. Initialize two counters `count1` and `count2`:

   * `count1`: number of changes required to match pattern starting with `'0'`
   * `count2`: number of changes required to match pattern starting with `'1'`

2. Iterate over the string:
   * At each index `i`:
     * Expected character for pattern 1: `'0'` if `i % 2 == 0`, else `'1'`
     * Expected character for pattern 2: `'1'` if `i % 2 == 0`, else `'0'`
     * Compare the actual character with both patterns and increment the respective counter if mismatch.

3. Return `min(count1, count2)`.

# Complexity Analysis

* **Time:** O(n) per string
* **Space:** O(1) (no extra space used)

#  Code

```java
public class Solution {
    public static int makeBeautiful(String str) {
        int count1 = 0; // mismatch with pattern starting with '0'
        int count2 = 0; // mismatch with pattern starting with '1'
        int n = str.length();
        
        for (int i = 0; i < n; i++) {
            char expected1 = (i % 2 == 0) ? '0' : '1';
            char expected2 = (i % 2 == 0) ? '1' : '0';
            
            if (str.charAt(i) != expected1) count1++;
            if (str.charAt(i) != expected2) count2++;
        }

        return Math.min(count1, count2);
    }
}
```

---

