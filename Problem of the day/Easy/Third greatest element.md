# Intuition

Sorting the entire array to find just the third largest element is unnecessary overhead. A direct scan tracking only the top three values is more efficient.

---

# Approach

Iterate once through the array and maintain three variables:
`first` (largest), `second` (second largest), and `third` (third largest).

For each element:

* If it's greater than `first`, shift values: `third = second`, `second = first`, `first = element`.
* Else if it's greater than `second`, shift: `third = second`, `second = element`.
* Else if it's greater than `third`, update `third = element`.

At the end of the traversal, `third` holds the third largest value.

---

# Complexity

* **Time complexity:** `O(N)` — single scan.
* **Space complexity:** `O(1)` — no extra storage proportional to input size.

---

# Code

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
    public static int findThirdLagrest(ArrayList<Integer> arr) {

        int first = Integer.MIN_VALUE;
        int second = Integer.MIN_VALUE;
        int third = Integer.MIN_VALUE;

        for (int num : arr) {

            if (num > first) {
                third = second;
                second = first;
                first = num;
            } 
            else if (num > second) {
                third = second;
                second = num;
            } 
            else if (num > third) {
                third = num;
            }
        }
        return third;
    }
};

```

---

# Example Walkthrough

Input:
`[2, 6, 7, 4, 9]`

| Element | first | second | third |
| ------- | ----- | ------ | ----- |
| 2       | 2     | -∞     | -∞    |
| 6       | 6     | 2      | -∞    |
| 7       | 7     | 6      | 2     |
| 4       | 7     | 6      | 4     |
| 9       | 9     | 7      | 6     |

Result → `third = 6`
