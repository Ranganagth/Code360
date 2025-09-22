# Intuition

We want to calculate:
```
(product of digits) - (sum of digits)
```
for the given number `N`.

Example: `N = 234`  
- Digits = `2, 3, 4`  
- Product = `2 * 3 * 4 = 24`  
- Sum = `2 + 3 + 4 = 9`  
- Answer = `24 - 9 = 15`

---

# Approach

1. Initialize:
   - `product = 1`  
   - `sum = 0`
2. Extract digits using modulo and division:
   - While `n > 0`:
     - digit = `n % 10`
     - product *= digit
     - sum += digit
     - n = n / 10
3. Return `product - sum`.

---

# Complexity

- **Time:** `O(d)` where `d` = number of digits in `n` (at most 6 for constraint `n ≤ 10^5`) → effectively constant.  
- **Space:** `O(1)` (just a few variables).  

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int findProductSumDifference(int n) {
        int product = 1;
        int sum = 0;

        while (n > 0) {
            int digit = n % 10;
            product *= digit;
            sum += digit;
            n /= 10;
        }

        return product - sum;
    }
}
```

---

## Walkthrough

Input: `4421`  
- Digits = `4, 4, 2, 1`  
- Product = `32`  
- Sum = `11`  
- Result = `32 - 11 = 21` 

---
