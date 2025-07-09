# Intuition

We need to perform **precise division** of `X / Y` and return the result as a **string with exactly `N` digits after the decimal point**, without rounding.

For example:
* `5 / 4 = 1.25` → for `N = 5`, return `"1.25000"`

Key points:
* Don’t round.
* Pad with zeros if result has fewer than `N` decimal digits.

---

# Approach

1. Use `Math.floor(x / y)` to get the integer part of the division.
2. Get the remainder: `remainder = abs(x % y)`, and work on computing decimals.
3. Multiply remainder by 10 for each decimal place and extract digits using division.
4. Accumulate the decimal digits in a string.
5. Combine integer part and decimal part with a `.`.
6. If the result is negative (i.e., X and Y have opposite signs), prefix the result with `-`.

---

# Complexity Analysis

* **Time Complexity:** `O(N)` → loop runs `N` times to compute `N` decimal digits.
* **Space Complexity:** `O(N)` → storing `N` characters in the decimal part.

---

# Code

```java
public class Solution {
    public static String findDivision(int x, int y, int n) {
        if (x == 0) {
            return "0." + "0".repeat(n);
        }

        boolean isNegative = (x < 0) ^ (y < 0);  // XOR: result is negative if signs differ

        long absX = Math.abs((long)x);
        long absY = Math.abs((long)y);

        long integerPart = absX / absY;
        long remainder = absX % absY;

        StringBuilder decimalPart = new StringBuilder();

        for (int i = 0; i < n; i++) {
            remainder *= 10;
            long digit = remainder / absY;
            decimalPart.append(digit);
            remainder %= absY;
        }

        StringBuilder result = new StringBuilder();
        if (isNegative) {
            result.append("-");
        }

        result.append(integerPart).append(".").append(decimalPart);
        return result.toString();
    }
}

```

---

### Example Walkthrough

### **Example 1:**

```text
Input: x = 5, y = 4, n = 5
```

* Integer part = `Math.floor(5 / 4) = 1`
* Remainder = `1`
* Now compute 5 decimal digits:

  * `1 * 10 / 4 = 2` → remainder 2
  * `2 * 10 / 4 = 5` → remainder 0
  * Rest = `0` → pad with 3 zeros

Result = `"1.25000"`

### **Example Usage**

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(Solution.findDivision(5, 4, 1));     // 1.2
        System.out.println(Solution.findDivision(5, 4, 5));     // 1.25000
        System.out.println(Solution.findDivision(-1, -2, 1));   // 0.5
        System.out.println(Solution.findDivision(1, -5, 6));    // -0.200000
        System.out.println(Solution.findDivision(-1, 1000, 2)); // -0.00
    }
}
```

---

## **Summary**

* We compute `N` decimal digits manually using remainder method (like long division).
* Ensure correct handling of signs and zero-padding
* Handles negative values correctly.
* Avoids rounding; computes only the first `N` digits.
* Works with large values using `long` to prevent overflow.
* Efficient for large `N` (up to `10^4`).

---
