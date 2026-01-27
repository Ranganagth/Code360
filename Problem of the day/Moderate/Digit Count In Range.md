# Intuition

You are asked to count **how many times a digit `K` appears** in **all numbers from `A` to `B` (inclusive)**.

* Each **occurrence at every digit place counts**
* `A` and `B` can be as large as **10¹⁸**, so iterating number by number is impossible
* This is a **digit DP / digit counting** problem

Key idea:

> Count how many times digit `K` appears from `0` to `B`,
> subtract how many times it appears from `0` to `A-1`.


## Core Insight

Let:

```
count(B)  = number of times digit K appears in [0, B]
count(A-1)= number of times digit K appears in [0, A-1]
```

Then:

```
answer = count(B) - count(A-1)
```

So the problem reduces to **counting digit occurrences from 0 to N efficiently**.

---
# Approach
### How to Count Digit Occurrences from `0` to `N`

We process **each digit position independently** (ones, tens, hundreds, …).

For a given position with factor `f = 1, 10, 100, ...`:

```
higher = N / (f * 10)
current = (N / f) % 10
lower = N % f
```

#### Rules for counting digit `K` at this position

1. **If K ≠ 0**

   * If `current > K`
     → `(higher + 1) * f`
   * If `current == K`
     → `higher * f + (lower + 1)`
   * If `current < K`
     → `higher * f`

2. **If K == 0** (special case — no leading zeros allowed)

   * Ignore numbers with leading zeros
   * Adjust count by subtracting one full block


## Algorithm

1. If `A > B`, swap them
2. Compute:

   ```
   count(B) - count(A - 1)
   ```
3. Use digit-position analysis for `count(N)`

---

# Complexity

- **Time Complexity:** `O(log₁₀ N)` per test case
	* Extremely fast even for `10¹⁸`

- **Space Complexity:** O(1)

---

# Code

```java
public class Solution {

    public static long digitCount(int K, long A, long B) {
        if (A > B) {
            long temp = A;
            A = B;
            B = temp;
        }
        return countUpTo(B, K) - countUpTo(A - 1, K);
    }

    private static long countUpTo(long N, int K) {
        if (N < 0) return 0;

        long count = 0;
        for (long factor = 1; factor <= N; factor *= 10) {
            long lower = N % factor;
            long current = (N / factor) % 10;
            long higher = N / (factor * 10);

            if (K != 0) {
                if (current > K) {
                    count += (higher + 1) * factor;
                } else if (current == K) {
                    count += higher * factor + (lower + 1);
                } else {
                    count += higher * factor;
                }
            } else {
                if (higher == 0) continue;

                if (current > 0) {
                    count += (higher - 1) * factor + factor;
                } else {
                    count += (higher - 1) * factor + (lower + 1);
                }
            }
        }
        return count;
    }
};

```

---

# Example Walkthrough

#### Input

```
K = 3
A = 1
B = 15
```

Numbers containing `3`:

```
3   → 1 time
13  → 1 time
```

Total occurrences = **2**

---

#### Input

```
K = 1
A = 1
B = 15
```

Occurrences:

```
1
10
11 (twice)
12
13
14
15
```

Total = **8**

---

### Key Takeaways

* Brute force is impossible due to large constraints
* Digit-position counting solves the problem optimally
* Special handling is required when `K = 0`
* This is a classic **number theory + digit math** problem
