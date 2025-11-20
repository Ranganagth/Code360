# Intuition

Keyboard is a permutation. Map each character to its index. Start at index 0. For each character in the message, add `abs(currentIndex − targetIndex)`, then move the finger to `targetIndex`.

---

# Approach

1. Create an array `pos[26]` storing the index of each character in `keyboard`.
2. Maintain `cur = 0`.
3. For each character `c` in `s`:
   • `t = pos[c]`
   • `ans += |cur − t|`
   • `cur = t`
4. Return the accumulated sum.

---

# Complexity

• **Time:** O(|S| + 26)
• **Space:** O(1)

---

# Code

```java
public class Solution {
    public static int timeTakenToMail(String keyboard, String s) {

        int[] pos = new int[26];
        for (int i = 0; i < 26; i++) {
            pos[keyboard.charAt(i) - 'a'] = i;
        }

        int cur = 0;
        int total = 0;

        for (int i = 0; i < s.length(); i++) {
            int t = pos[s.charAt(i) - 'a'];
            total += Math.abs(cur - t);
            cur = t;
        }

        return total;
    }
}
```

---

# Walkthrough Example

**keyboard**: `"qwertyuiopasdfghjklzxcvbnm"`
s: `"perry"`

• Start `cur = 0`
• `p` → index 9 → +9 → cur = 9
• `e` → index 2 → +7 → cur = 2
• `r` → index 3 → +1 → cur = 3
• `r` → index 3 → +0 → cur = 3
• `y` → index 5 → +2 → cur = 5

**Total = 19**
