# Intuition

We need to construct a **new number** by looking at every bit position across all numbers in the array:

* If **more than half** of the numbers have that bit set (`1`), then the new number’s bit is `1`.
* Otherwise, it’s `0`.

This is like doing a **majority voting per bit position**.

---

# Approach

1. Find the maximum number in the array to know how many bits we need to check.

   * Example: if max = 9 (binary `1001`), we only need up to 4 bits.
2. For each bit position `b`:

   * Count how many numbers have that bit set.
   * If count > n/2 → set that bit in the result.
3. Construct the final number using bitwise operations.

---

# Complexity

* **Time:** O(N \* log(max(arr))).
  Since each number is checked across its bits.
  For N ≤ 10^5 and log(max(arr)) ≤ 32, this is efficient.
* **Space:** O(1).

---

# Code

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {

    public static int getNewNum(ArrayList<Integer> arr, int n) {
        int result = 0;

        // Find the max value to know bit length
        int maxVal = 0;
        for (int num : arr) {
            maxVal = Math.max(maxVal, num);
        }

        int maxBits = (maxVal == 0) ? 1 : (32 - Integer.numberOfLeadingZeros(maxVal));

        for (int bit = 0; bit < maxBits; bit++) {
            int countSet = 0;

            for (int num : arr) {
                if ((num & (1 << bit)) != 0) {
                    countSet++;
                }
            }

            // If set bits are more than unset bits → set in result
            if (countSet > n - countSet) {
                result |= (1 << bit);
            }
        }

        return result;
    }
}
```

---

## Example Walkthrough

Input: `arr = [8, 5, 10]`

* Binary:

  ```
  8  = 1000
  5  = 0101
  10 = 1010
  ```
* Bitwise majority check:

  * bit0: \[0,1,0] → set=1, unset=2 → majority=0
  * bit1: \[0,0,1] → set=1, unset=2 → majority=0
  * bit2: \[0,1,0] → set=1, unset=2 → majority=0
  * bit3: \[1,0,1] → set=2, unset=1 → majority=1
* Result = `1000` = 8. 

---

