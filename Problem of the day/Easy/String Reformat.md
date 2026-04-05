# Intuition

Hyphens are irrelevant. Only characters matter.
Grouping must happen from the **end** so that all groups (except possibly the first) have exactly `K` characters.
Right-to-left construction avoids computing first group size explicitly.

---

# Approach

1. Traverse string from right to left.
2. Ignore `'-'`.
3. Convert characters to uppercase and append to result.
4. Maintain a counter:

   * After every `K` characters → insert `'-'`.
5. Remove trailing `'-'` if added.
6. Reverse the result.

---

# Complexity

- **Time:**
$$O(N)$$


- **Space:**
$$O(N)$$


---

# Code (Java)

```java
public class Solution {

    public static String stringReformat(String s, int k) {

        StringBuilder sb = new StringBuilder();
        int count = 0;

        for (int i = s.length() - 1; i >= 0; i--) {

            char ch = s.charAt(i);

            if (ch == '-') continue;

            sb.append(Character.toUpperCase(ch));
            count++;

            if (count == k) {
                sb.append('-');
                count = 0;
            }
        }

        // Remove trailing dash if exists
        if (sb.length() > 0 && sb.charAt(sb.length() - 1) == '-') {
            sb.deleteCharAt(sb.length() - 1);
        }

        return sb.reverse().toString();
    }
}
```

---

# Example Walkthrough

### Input

```
S = "Ab-ijklmno-pqr", K = 3
```

### Step 1: Remove hyphens + uppercase (conceptual)

```
ABJKLMNOPQR
```

### Step 2: Build from right

| Step | Char | Builder (reverse order) | Count |
| ---- | ---- | ----------------------- | ----- |
| r    | R    | R                       | 1     |
| q    | Q    | RQ                      | 2     |
| p    | P    | RQP-                    | 0     |
| o    | O    | RQP-O                   | 1     |
| n    | N    | RQP-ON                  | 2     |
| m    | M    | RQP-ONM-                | 0     |
| l    | L    | RQP-ONM-L               | 1     |
| k    | K    | RQP-ONM-LK              | 2     |
| j    | J    | RQP-ONM-LKJ-            | 0     |
| i    | I    | RQP-ONM-LKJ-I           | 1     |
| b    | B    | RQP-ONM-LKJ-IB          | 2     |
| a    | A    | RQP-ONM-LKJ-IBA-        | 0     |

Remove trailing `-`

### Step 3: Reverse

```
ABI-JKL-MNO-PQR
```

---

# Key Insight

- Right-to-left grouping eliminates special handling for the first group

