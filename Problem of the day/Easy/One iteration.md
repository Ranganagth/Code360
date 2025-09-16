# Intuition

We need the **maximum sum of two elements** in the array, but we can traverse the array only once.
That means:

* We just need the **largest element** and the **second largest element** in the array.
* Their sum will be the maximum possible sum of two distinct elements.

So, as we traverse:

* Maintain two variables:

  * `firstMax` (largest so far)
  * `secondMax` (second largest so far)
* Update them on the fly for each element.

---

# Approach

1. Initialize `firstMax` and `secondMax` with very small values (e.g. `Long.MIN_VALUE` since values can be as small as -1e9).
2. Traverse the array once:

   * If current number > `firstMax` → update `secondMax = firstMax`, then `firstMax = current`.
   * Else if current number > `secondMax` → update `secondMax = current`.
3. At the end, return `firstMax + secondMax`.

---

# Complexity

* **Time:** `O(N)` (single pass).
* **Space:** `O(1)` (constant extra variables).

---

# Code
### Solution 1

```java
import java.util.* ;
import java.io.*; 

public class Solution {

    public static int oneIteration(int[] A) {
        long firstMax = Long.MIN_VALUE;
        long secondMax = Long.MIN_VALUE;

        for (int num : A) {
            if (num > firstMax) {
                secondMax = firstMax;
                firstMax = num;
            } else if (num > secondMax) {
                secondMax = num;
            }
        }

        return (int)(firstMax + secondMax);
    }
}
```

### Solution 2


```java
import java.util.* ;
import java.io.*; 
public class Solution {

    public static int oneIteration(int[] A) {
        
        int max1 = Integer.MIN_VALUE;
		int max2 = Integer.MIN_VALUE;

		for (int num : A) {
			if (num > max1) {
				max2 = max1;
				max1 = num;
			} else if (num > max2) {
				max2 = num;
			}
		}

		return max1 + max2;
    }
}

```
---

## Example Walkthrough

Input:
`A = [6, 1, 5, 7]`

* Start: `firstMax = -inf, secondMax = -inf`
* See 6 → `firstMax=6, secondMax=-inf`
* See 1 → `firstMax=6, secondMax=1`
* See 5 → `firstMax=6, secondMax=5`
* See 7 → `firstMax=7, secondMax=6`

Answer = `7+6 = 13`.

---
