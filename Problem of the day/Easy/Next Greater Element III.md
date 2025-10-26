# Intuition

We need to find the **next greater permutation** of the digits of a given number.
If such a rearrangement doesn’t exist (i.e., digits are in descending order), return `-1`.

Example:

* `225 → 252` True
* `66 → -1` False
* `342 → 423` True
* `39 → 93` True

---

# Approach

We’ll apply the **next permutation algorithm** to the digits of the number.

### Step 1: Convert the number into a digit array

We can easily manipulate digits by converting the number to a `char[]` or `int[]`.

Example:
`n = 225 → digits = [2, 2, 5]`

### Step 2: Find the “break point”

Find the **rightmost index `i`** such that:

```
digits[i] < digits[i + 1]
```

* This means `digits[i]` is where ascending order breaks from the right side.
* If no such index exists → digits are in descending order → return `-1`.

Example:

```
digits = [2, 2, 5]
i = 1 (because 2 < 5)
```

### Step 3: Find the smallest digit to the right of `i` that’s **greater** than `digits[i]`

Swap that digit with `digits[i]`.

Example:

```
[2, 2, 5]
Swap digits[1] and digits[2] → [2, 5, 2]
```

### Step 4: Reverse the digits after index `i`

To make the number as **small as possible** after increasing it.

Example:

```
After swap: [2, 5, 2]
Reversing after index 1 → [2, 5, 2] (no change here)
Final: 252
```

### Step 5: Convert digits back to number

Return the result as a `long`.

---

# Complexity

| Type  | Complexity                        |
| ----- | --------------------------------- |
| Time  | O(d) (where d = number of digits) |
| Space | O(d) (for digit array)            |

---

# Code

```java
import java.util.*;

public class Solution {
    public static long bobsHomework(int n) {
        char[] digits = String.valueOf(n).toCharArray();
        int len = digits.length;

        // Step 1: find the rightmost "break point"
        int i = len - 2;
        while (i >= 0 && digits[i] >= digits[i + 1]) {
            i--;
        }

        if (i < 0) {
            // digits are in descending order, no greater permutation
            return -1;
        }

        // Step 2: find the smallest digit greater than digits[i] to the right
        int j = len - 1;
        while (j > i && digits[j] <= digits[i]) {
            j--;
        }

        // Step 3: swap
        char temp = digits[i];
        digits[i] = digits[j];
        digits[j] = temp;

        // Step 4: reverse the right part
        reverse(digits, i + 1, len - 1);

        // Step 5: convert back to number
        long result = Long.parseLong(new String(digits));
        return result;
    }

    private static void reverse(char[] arr, int left, int right) {
        while (left < right) {
            char temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
};

```

---

## Example Walkthrough

**Example 1:**
Input: `225`

```
digits = [2, 2, 5]
i = 1 (since 2 < 5)
Find j = 2 (5 > 2)
Swap → [2, 5, 2]
Reverse after i → [2, 5, 2]
Result = 252
```

Output: `252`

**Example 2:**
Input: `66`

```
digits = [6, 6]
No i found where digits[i] < digits[i + 1]
Return -1
```

Output: `-1`

**Example 3:**
Input: `342`

```
digits = [3, 4, 2]
i = 0 (3 < 4)
Find j = 1 (4 > 3)
Swap → [4, 3, 2]
Reverse after 0 → [4, 2, 3]
Result = 423
```

Output: `423`

---

## Summary

* Time Efficient (O(d))
* Works for up to `2 * 10^9`
* Same logic as `next_permutation` from C++ STL.

---
