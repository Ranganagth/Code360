# Intuition

Maximum XOR occurs when two numbers differ at the **highest possible bit**.

Key observation:

* XOR is maximized when bits differ from the most significant differing bit onward.
* Find the **leftmost bit where L and R differ**
* From that bit onward, all lower bits can be made 1 in result

Thus:
If $(L \oplus R)$ has highest set bit at position (k),
Answer = number with all bits = 1 from 0 to $k → (2^{k+1} - 1)$

---

# Approach

1. Compute `xor = L ^ R`
2. Find position of most significant set bit in `xor`
3. Construct result:

   * Set all bits from 0 to that position → all 1’s
4. Return result

Efficient way:

* Keep shifting `xor`
* Build result as `(result << 1) | 1`

---

# Complexity

* **Time complexity:**  $$O(\log N)$$

* **Space complexity:**  $$O(1)$$

---

# Code

```Java
import java.util.* ;
import java.io.*; 
public class Solution {

    public static int maxXor(int L, int R) {
        int xor = L ^ R;

        int maxXor = 0;
        while (xor > 0) {
            maxXor = (maxXor << 1) | 1;
            xor >>= 1;
        }
        return maxXor;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine().trim());
        while (T-- > 0) {
            String[] range = br.readLine().trim().split(" ");
            int L = Integer.parseInt(range[0]);
            int R = Integer.parseInt(range[1]);
            System.out.println(maxXor(L, R));
        }
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

L = 8 → 01000
R = 20 → 10100

L ^ R = 11100

Highest differing bit at position 4

Construct:
11111 → 31

Answer = 31

## Example 2:

L = 1 → 001
R = 3 → 011

L ^ R = 010

Highest differing bit at position 1

Construct:
011 → 3

Answer = 3

