# Intuition

* Just like converting decimal to binary or hexadecimal, we repeatedly divide the number by the base (`58` here).
* Collect remainders at each step, map them to the **Base58 alphabet**, and then reverse the sequence.
* Special case: if `n == 0`, directly return `"1"` (since 0 maps to the first character in Base58 alphabet).

---

# Approach

1. Define the Base58 alphabet:

   ```
   "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
   ```

   * Index `0` → `'1'`, index `1` → `'2'`, ..., index `57` → `'z'`.
2. While `n > 0`:

   * Take remainder `n % 58`.
   * Append alphabet\[remainder].
   * Update `n = n / 58`.
3. Reverse the collected characters to get the final answer.
4. Handle `n == 0` explicitly.

---

# Complexity

* Each step reduces `n` by a factor of 58.
* Time complexity: **O(log₅₈ N)** ≈ O(log N).
* Space complexity: **O(log₅₈ N)** (for storing result string).

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    private static final String BASE58_ALPHABET = 
        "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz";

    public static String encodeBase58(int n){
        if (n == 0) return "1";  // special case for zero

        StringBuilder sb = new StringBuilder();
        while (n > 0) {
            int remainder = n % 58;
            sb.append(BASE58_ALPHABET.charAt(remainder));
            n /= 58;
        }
        return sb.reverse().toString();
    }

    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        while (t-- > 0) {
            int n = sc.nextInt();
            System.out.println(encodeBase58(n));
        }
    }
}
```

---

### Walkthrough with Example

Input:

```
2
10
67
```

* For `10`:

  * 10 % 58 = 10 → alphabet\[10] = `'B'`.
  * 10 / 58 = 0 → stop.
    Result = `"B"`.

* For `67`:

  * 67 % 58 = 9 → `'A'`.
  * 67 / 58 = 1 → remainder 1 → `'2'`.
    Collect = `"A2"` → reverse = `"2A"`.

Output:

```
B
2A
```

---
