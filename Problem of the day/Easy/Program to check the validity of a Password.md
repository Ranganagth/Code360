# Intuition

Validity depends on four mandatory character-types and strict structural rules. A single scan is enough. Track presence of digit, lowercase, uppercase, and special character while rejecting space and invalid length up-front.

---

# Approach

1. Reject immediately if length < 8 or length > 15.
2. Scan each character:
   • If space → invalid.
   • If digit → mark digit = true.
   • If lowercase → mark lower = true.
   • If uppercase → mark upper = true.
   • Else treat as special character → mark special = true.
3. At end, all four flags must be true.

---

# Complexity

- **Time:** O(n)
- **Space:** O(1)

---

# Code

```java
public class Solution {
    public static boolean isValid(String str) {

        int n = str.length();
        if (n < 8 || n > 15) return false;

        boolean digit = false;
        boolean lower = false;
        boolean upper = false;
        boolean special = false;

        for (char c : str.toCharArray()) {
            if (c == ' ') return false;

            if (c >= '0' && c <= '9') digit = true;
            else if (c >= 'a' && c <= 'z') lower = true;
            else if (c >= 'A' && c <= 'Z') upper = true;
            else special = true;
        }

        return digit && lower && upper && special;
    }
};

```

---

# Example Walkthrough

**Input:** `"CODiNGNinja+1"`
Length ok.

**Scan:** contains uppercase, lowercase, digit ‘1’, special ‘+’, no spaces.

All required categories present -> **Valid**.
