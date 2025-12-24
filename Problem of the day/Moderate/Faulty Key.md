# Intuition

Binary addition does not require `+`.
For each bit position:

* XOR (`^`) gives the sum **without carry**
* AND (`&`) gives the carry bits
* Carry must be shifted left by one bit

Repeat until there is no carry left. That final value is the sum.

This is exactly how hardware adders work.

---

# Approach

1. Compute partial sum using `a ^ b`
2. Compute carry using `(a & b) << 1`
3. Replace `a` with partial sum, `b` with carry
4. Repeat until `b == 0`
5. Return `a`

No `+`, no `++`, no arithmetic addition.

---

# Complexity

* **Time:** `O(1)` (bounded by number of bits, max 32 iterations)
* **Space:** `O(1)`

---

# Code

```java
public class Solution {
    public static int getSum(int a, int b) {
        while (b != 0) {
            int carry = (a & b) << 1;
            a = a ^ b;
            b = carry;
        }
        return a;
    }
};

```

---

# Example Walkthrough

**Input:** `a = 4`, `b = 6`
Binary:

* `4 = 0100`
* `6 = 0110`

Iteration 1:

* `sum = 0100 ^ 0110 = 0010` (2)
* `carry = (0100 & 0110) << 1 = 1000` (8)

Iteration 2:

* `sum = 0010 ^ 1000 = 1010` (10)
* `carry = (0010 & 1000) << 1 = 0000`

Carry is zero → stop
**Result =** `10`

---

> This works for positive, negative, and mixed-sign integers due to two’s complement representation.
