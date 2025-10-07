# Intuition

We need to count all numbers ≤ `N` whose digits consist of **only `0` and `1`**.
These are numbers like:

```
1, 10, 11, 100, 101, 110, 111, 1000, ...
```

If you notice — these numbers look like **binary numbers interpreted in decimal**.

For example:

| Binary | Decimal |
| ------ | ------- |
| 1      | 1       |
| 10     | 10      |
| 11     | 11      |
| 100    | 100     |
| 101    | 101     |
| 110    | 110     |
| 111    | 111     |
| 1000   | 1000    |

We can see that as the length grows, the numbers grow exponentially.
So, instead of iterating through all integers from `1` to `N` (which is impossible for `N` up to 10⁹),
we can **generate** numbers that only contain `0` and `1` using BFS or DFS until they exceed `N`.

---

# Approach

1. **Use BFS (Queue) starting from `1`.**

   * Push `"1"` initially.
   * Repeatedly pop a number `x`, convert it to `long`.
   * If `x <= N`, count it.
   * Then push `x * 10` (append `0`) and `x * 10 + 1` (append `1`) into the queue.

2. Stop generating when the next number exceeds `N`.

This ensures we **only generate valid numbers**, not iterate over all integers.

---

# Complexity Analysis

* Each valid number generates at most 2 more numbers.
* The total numbers generated ≈ number of valid “binary-decimal” numbers ≤ N.
* For N ≤ 10⁹, there are at most numbers up to `"1111111111"` (10 digits in binary pattern).
* So, at most ~1023 numbers generated (2¹⁰ - 1).

Hence,
**Time Complexity:** `O(log N)` (very small constant)
**Space Complexity:** `O(log N)`

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static long countOfNumbers(long n) {
        Queue<Long> q = new LinkedList<>();
        q.add(1L);
        long count = 0;

        while (!q.isEmpty()) {
            long num = q.poll();

            if (num > n) continue; // Ignore if it exceeds N

            count++; // Valid number

            long next0 = num * 10;     // Append 0
            long next1 = num * 10 + 1; // Append 1

            if (next0 <= n) q.add(next0);
            if (next1 <= n) q.add(next1);
        }

        return count;
    }
}
```

---
## Example

For `N = 21`:

* Start with `1`

  * Count = 1
  * Push `10`, `11`
* Next: `10`

  * Count = 2
  * Push `100`, `101`
* Next: `11`

  * Count = 3
  * Push `110`, `111`
* Next: `100` → exceeds 21, stop.

Answer = `3` (1, 10, 11)

### Walkthrough

**Input:**

```
N = 250
```

Generated sequence:
`1, 10, 11, 100, 101, 110, 111`

All ≤ 250 → count = 7.

Output: `7`

---
