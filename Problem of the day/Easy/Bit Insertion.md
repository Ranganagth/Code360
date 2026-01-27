# Intuition

You are asked to **insert bits of `Y` into `X` from bit position `A` to `B` (inclusive)**.

“Inserting” means two clear steps:

1. **Clear bits A through B in X**
2. **Shift Y so that its least significant bit aligns with position A**
3. **OR the shifted Y into X**

Bit positions are **0-indexed** and numbers are **32-bit**.

---

### Key Bit Manipulation Ideas

* To clear bits from `A` to `B`, build a **mask** that has:

  * `0` in range `[A, B]`
  * `1` everywhere else
* Then:
  `X_cleared = X & mask`
* Shift `Y` left by `A` positions:
  `Y_shifted = Y << A`
* Final result:
  `result = X_cleared | Y_shifted`

### Step-by-Step Mask Construction

1. **Left mask** → bits `B+1` and above are `1`

   ```java
   int leftMask = ~0 << (b + 1);
   ```

2. **Right mask** → bits `A-1` and below are `1`

   ```java
   int rightMask = (1 << a) - 1;
   ```

3. **Combined mask**

   ```java
   int mask = leftMask | rightMask;
   ```

This clears bits from `A` to `B`.

---
# Approach

### Algorithm

1. Create a mask to clear bits `[A, B]` in `X`
2. Clear those bits
3. Shift `Y` left by `A`
4. OR the two values
5. Return the result

---

# Complexity

- **Time Complexity**
	* **O(1)** per test case
	  (only constant-time bit operations)

- **Space Complexity**
	* **O(1)**

---

# Code

```java
public class Solution {
    public static int bitInsertion(int x, int y, int a, int b) {

        int leftMask = (b < 31) ? (~0 << (b + 1)) : 0;
        int rightMask = (1 << a) - 1;
        int mask = leftMask | rightMask;

        int clearedX = x & mask;
        int shiftedY = y << a;

        return clearedX | shiftedY;
    }
};

```

---

# Example Walkthrough

#### Example

```
X = 1536, Y = 7, A = 1, B = 5
```

Binary:

```
X = 11000000000
Y =      0111
```

1. Clear bits 1 to 5 in X:

```
11000000000
↓
11000000000   (those bits were already 0)
```

2. Shift Y left by A = 1:

```
0111 << 1 = 01110
```

3. OR result:

```
11000000000
00000001110
------------
11000001110
```

Decimal result:

```
1550
```

---

### Final Insight

* This is a **classic bit-masking problem**
* Correct mask creation is the key
* Safe, fast, and works for all 32-bit integers
