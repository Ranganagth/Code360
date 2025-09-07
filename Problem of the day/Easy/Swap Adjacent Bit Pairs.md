# Intuition

- You are given an integer `N`. Its **binary representation** has bits indexed from right to left:  
  - LSB (Least Significant Bit) = position 1 → odd  
  - Next bit = position 2 → even  
  - And so on.  

- The task: **swap every even-positioned bit with its adjacent odd-positioned bit (to its right)**.

Example:  
`N = 45 → binary = 101101`  
Positions:  
```
bit6 bit5 bit4 bit3 bit2 bit1
  1     0     1     1     0     1
 even odd even odd even odd
```
After swapping adjacent pairs:  
```
bit6<->bit5, bit4<->bit3, bit2<->bit1
```
Result = `011110` (binary) = 30.

---

# Approach

1. **Use bitmasks to separate even and odd bits.**
   - Odd bitmask: `0x55555555` (binary `0101...` → picks odd-positioned bits).  
   - Even bitmask: `0xAAAAAAAA` (binary `1010...` → picks even-positioned bits).  

2. **Extract even and odd bits from `n`.**
   - `evenBits = n & 0xAAAAAAAA`  
   - `oddBits  = n & 0x55555555`

3. **Shift them towards each other.**
   - Shift even bits right: `evenBits >> 1`  
   - Shift odd bits left: `oddBits << 1`

4. **Combine results with OR.**
   - `result = (evenBits >> 1) | (oddBits << 1)`

---

# Complexity

- Each operation is **O(1)** (bitwise ops).  
- For `T` test cases, complexity = **O(T)**.  
- Space = **O(1)**.  

Perfectly efficient for constraints `T ≤ 10^5, N ≤ 10^9`.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int swapAdjacent(int n) {
        // Mask for odd and even bits
        int oddMask  = 0x55555555; // 0101...
        int evenMask = 0xAAAAAAAA; // 1010...

        // Extract odd and even bits
        int oddBits  = n & oddMask;
        int evenBits = n & evenMask;

        // Shift them
        oddBits <<= 1;
        evenBits >>= 1;

        // Combine
        return oddBits | evenBits;
    }
}
```

---

## Example Walkthrough

Input: `N = 9`  
- Binary = `1001` (bits: `1 0 0 1`)  
- Odd bits (mask 0101): `0001`  
- Even bits (mask 1010): `1000`  
- Shift: odd→`0010`, even→`0100`  
- Combine: `0110` = 6  

Output = **6** 

---
