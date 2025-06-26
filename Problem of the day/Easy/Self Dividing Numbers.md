# Intuition:
To solve the **Self-Dividing Numbers** problem, the key idea is to check **every number in the range** and determine if each digit of the number **divides the number completely (without remainder)** and does **not contain the digit 0**.

---

# Approach:

1. **Iterate from `lower` to `upper`**:
   * For each number, extract its digits.
   * If any digit is 0 or the number is not divisible by that digit → **not self-dividing**.
   * If it passes for all digits → **it's a valid self-dividing number**.
   
2. Add valid numbers to the result list and return.

---

# Code:

```java
import java.util.ArrayList;

public class Solution {
    public static ArrayList<Integer> findAllSelfDividingNumbers(int lower, int upper) {
        ArrayList<Integer> result = new ArrayList<>();

        for (int num = lower; num <= upper; num++) {
            if (isSelfDividing(num)) {
                result.add(num);
            }
        }

        return result;
    }

    private static boolean isSelfDividing(int num) {
        int temp = num;
        
        while (temp > 0) {
            int digit = temp % 10;

            if (digit == 0 || num % digit != 0) {
                return false;
            }

            temp /= 10;
        }

        return true;
    }
}
```

---

### Example:

For input `lower = 10`, `upper = 30`, this function returns:

```
[11, 12, 15, 22, 24]
```

which matches the expected self-dividing numbers within that range.

---
