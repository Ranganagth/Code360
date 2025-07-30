# Intuition

To find the total number of subarrays consisting **only of 1s** and **only of 0s**, we can **count the lengths of continuous segments of 1s and 0s**.
For each such segment of length `len`, the number of subarrays formed is:

```
(len * (len + 1)) / 2
```

This is because:

* A segment of length 3 → subarrays: [x], [x,x], [x,x,x] → 3 + 2 + 1 = 6 = (3\*4)/2.

---

# Approach

1. Traverse the array while maintaining a `count` of consecutive same elements (0 or 1).
2. If the current element breaks the streak, use the formula `(count * (count + 1)) / 2` to add to the total.
3. Reset the counter and continue.
4. After the loop, make sure to add the last segment count as well.

Repeat the above for each test case.

---

# Complexity

* **Time complexity:**
  $O(N)$ per test case

* **Space complexity:**
  $O(1)$ (constant extra space)

---

# Code

```java
import java.util.*;

public class Solution {
    public static int numberofSubarrays(int n, int[] arr) {
        int total = 0;
        int count = 1;

        for (int i = 1; i < n; i++) {
            if (arr[i] == arr[i - 1]) {
                count++;
            } else {
                total += (count * (count + 1)) / 2;
                count = 1;
            }
        }

        total += (count * (count + 1)) / 2; // for last segment
        return total;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {
            int n = sc.nextInt();
            int[] arr = new int[n];

            for (int i = 0; i < n; i++) {
                arr[i] = sc.nextInt();
            }

            System.out.println(numberofSubarrays(n, arr));
        }
    }
}
```
