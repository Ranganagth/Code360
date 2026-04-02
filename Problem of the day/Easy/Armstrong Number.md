# Intuition

An Armstrong number satisfies:

$$sum of (each digit ^ total_digits) == number$$

Example:

$$371 → 3 digits
3³ + 7³ + 1³ = 371
$$
---

# Approach

1. Count number of digits `k`
2. Traverse digits again:
   * compute `(digit ^ k)`
   * add to sum
3. Compare sum with original number

---

# Complexity

* **Time complexity:**
$$O(d)
$$
(where d = number of digits)

* **Space complexity:**
$$O(1)
$$
---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution 
{
	public static boolean isArmstrong(int num)
	{
	    int original = num;

	    // count digits
	    int k = String.valueOf(num).length();

	    int sum = 0;

	    while (num > 0) {
	        int digit = num % 10;

	        sum += Math.pow(digit, k);

	        num /= 10;
	    }

	    return sum == original;
	}
};

```

---

# Example walkthrough

Input:

```
153
```

```
digits = 3
1³ + 5³ + 3³ = 1 + 125 + 27 = 153
```

Output:

```
true
```
