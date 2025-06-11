> (Partially Accepted) Test cases: 11/12
# Intuition

The function `magic(P, Q)` simulates bitwise addition without using the `+` operator. It mimics the logic of binary addition:

* `A = P & Q` represents the **carry**.
* `B = P ^ Q` represents the **sum without carry**.
* Then, the carry is shifted left and added in the next iteration.

The loop continues until there’s no more carry (`Q == 0`).

So, the number of iterations in the while loop is equal to the number of carry propagations required to complete the bitwise addition.

# Approach

1. Convert binary strings `P` and `Q` to integers.
2. Use a `while` loop to simulate the logic:

   * `A = P & Q` (carry)
   * `B = P ^ Q` (sum)
   * `Q = A << 1`
   * `P = B`
3. Count the number of iterations until `Q` becomes 0.

# Complexity Analysis

* **Time complexity**: O(N), where N is the number of bits in the longest binary string. Each bit can contribute to at most one carry operation.
* **Space complexity**: O(1), only integers and a counter are used.

# Code

```java
import java.math.BigInteger; // Required for BigInteger operations

public class Solution {
    public static int countLoop(String pBinary, String qBinary) {
        // Convert binary strings to BigInteger
        BigInteger P = new BigInteger(pBinary, 2);
        BigInteger Q = new BigInteger(qBinary, 2);

        int count = 0; // Initialize loop counter

        // Continue looping as long as Q is greater than zero
        while (Q.compareTo(BigInteger.ZERO) > 0) {
            // A = P AND Q: This calculates the common set bits (carries)
            BigInteger A = P.and(Q);
            
            // B = P XOR Q: This calculates the sum bits without considering carries
            BigInteger B = P.xor(Q);
            
            // Q = A * 2 (or A << 1): The carries are shifted left by one position
            // to be added in the next iteration
            Q = A.shiftLeft(1);
            
            // P = B: The sum bits become the new P
            P = B;
            
            count++; // Increment loop counter
        }
        
        return count; // Return the total number of iterations
    }
}
```

---

### **Example Execution**

#### Input:

`P = "110"` (6), `Q = "10"` (2)
Iterations:

* 1st: P = 4, Q = 4
* 2nd: P = 0, Q = 8
* 3rd: P = 8, Q = 0
  **Output: 3**