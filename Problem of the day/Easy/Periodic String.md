# Intuition

A string `S` is **periodic** if it can be constructed by repeating a **smaller substring** multiple times.
For example:

* `"ababab"` → repeats `"ab"`
* `"aabbaabbaabb"` → repeats `"aabb"`

So, we need to find if such a substring exists whose repetition forms the original string.

---

## Approach 1 — Simple but Inefficient (O(n²))

For each possible period length `p` (1 to n/2):

* If `n % p == 0`, take substring `S[0:p]`.
* Repeat it `(n / p)` times and check if it equals `S`.

This works but is **too slow** for n up to `10^5`.

---

## Approach 2 — Efficient Trick (O(n)) using String Concatenation

Here’s a neat observation:

If you double the string (`S + S`),
and remove the **first and last characters**,
the original string `S` will appear inside this new string **if and only if** `S` is periodic.

### Why does this work?

* Let’s take `"ababab"`
  `S + S = "abababababab"`
  Removing first and last → `"bababababa"`
  `"ababab"` appears inside it → means periodic.

* `"abc"`
  `S + S = "abcabc"` → removing first and last `"bcab"`
  `"abc"` does not appear → not periodic.

So, we just check:

```java
if ((S + S).substring(1, 2 * S.length() - 1).contains(S))
    return true;
else
    return false;
```

This works in **O(n)** and is elegant.

---

# Complexity Analysis

* **Time:** O(n)
* **Space:** O(n)
* Works efficiently for up to 10⁵ characters.

---

## Example Walkthrough

**Input:** `"aabbaaabba"`

`S + S = "aabbaaabbaaabbaaabba"`

Remove first and last char → `"abbaaabbaaabbaaabb"`

Now `"aabbaaabba"` appears in it → Periodic.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static boolean isPeriodic(String s) {
        int n = s.length();
        if (n <= 1) return false;
        
        String doubled = s + s;
        String sub = doubled.substring(1, 2 * n - 1);
        return sub.contains(s);
    }
};

```

---

## Example Verification

**Input:**

```
3
xxxxxx
aabbaaabba
abcba
```

**Output:**

```
True
True
False
```

Matches expected output.

---
