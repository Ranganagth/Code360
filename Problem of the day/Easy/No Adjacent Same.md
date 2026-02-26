# Intuition

A number is valid if **no two adjacent bits are the same** in its binary representation (from the leftmost set bit onward).

If two adjacent bits are equal, then:

```
00  or  11
```

Using bit manipulation:

```
n ^ (n >> 1)
```

* If adjacent bits are different → XOR gives `1`
* If adjacent bits are same → XOR gives `0`

For a valid alternating bit pattern, the XOR result must be a sequence of all `1`s.

A number consisting of all `1`s satisfies:

$$x & (x+1) = 0$$

---

# Approach

1. Compute:

   ```
   x = n ^ (n >> 1)
   ```
2. Check whether `x` is all `1`s:

   ```
   (x & (x + 1)) == 0
   ```
3. Return true if condition holds.

---

# Complexity

* **Time:** (O(1))
* **Space:** (O(1))

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static boolean checkBits(int n) {

        int x = n ^ (n >> 1);

        return (x & (x + 1)) == 0;
    }
};

```

---

# Example walkthrough with explanation

Example:

```
n = 21
binary = 10101
```

Step:

```
n >> 1 = 01010

x = 10101 ^ 01010 = 11111
```

Check:

```
11111 & 100000 = 0
```

Valid → `true`.

Example:

```
n = 31
binary = 11111
```

```
n >> 1 = 01111
x = 10000
```

```
10000 & 10001 != 0
```

Invalid → `false`.
