# Intuition

Brute force counting bits from 1 to N is $(O(N \log N))$, too slow.

Observe pattern:

* Bits repeat in cycles of length $(2^{i+1})$
* For each bit position (i):

  * In every cycle, $(2^i)$ numbers have that bit set

Count contribution of each bit independently.

---

# Approach

For each bit position (i) (0 to 30):

1. Cycle length = $(2^{i+1})$
2. Full cycles:
   $$
   \text{fullCycles} = \frac{N + 1}{2^{i+1}}
   $$
3. Contribution from full cycles:
   $$
   \text{fullCycles} \times 2^i
   $$
4. Remaining part:
   $$
   \text{remaining} = (N + 1) \mod 2^{i+1}
   $$
5. Extra set bits:
   $$
   \max(0, \text{remaining} - 2^i)
   $$
6. Add all contributions modulo $(10^9 + 7)$

---

# Complexity

* **Time complexity:**
  $$O(\log N)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
public class Solution 
{
    public static int countSetBits(int n) 
    {
        long mod = 1000000007;
        long ans = 0;

        for (int i = 0; i < 31; i++) {

            long bit = 1L << i;
            long cycle = bit << 1;

            long fullCycles = (n + 1L) / cycle;
            long count = fullCycles * bit;

            long remaining = (n + 1L) % cycle;
            count += Math.max(0, remaining - bit);

            ans = (ans + count) % mod;
        }

        return (int) ans;
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

N = 5

Binary:
1 → 001
2 → 010
3 → 011
4 → 100
5 → 101

Bit contributions:

Bit 0:

* Pattern: 0,1 alternating
* Count = 3

Bit 1:

* Pattern: 0,0,1,1
* Count = 2

Bit 2:

* Pattern: 0,0,0,0,1,1
* Count = 2

Total = 3 + 2 + 2 = 7


## Example 2:

N = 10

Compute per bit:

* Bit 0 → 5
* Bit 1 → 4
* Bit 2 → 4
* Bit 3 → 4

Total = 17
