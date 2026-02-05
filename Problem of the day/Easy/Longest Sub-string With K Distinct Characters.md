# Intuition

You need the **longest contiguous substring** that contains **at most `K` distinct characters**.

Key observations:

* Substring ⇒ characters must be **contiguous**
* “At most K distinct characters” ⇒ once distinct count exceeds `K`, the substring becomes invalid
* Brute force (checking all substrings) is too slow for `N = 10⁴`

This naturally fits a **sliding window (two pointers)** approach:

* Expand the window to the right
* Track character frequencies
* If distinct characters exceed `K`, shrink from the left
* Keep updating the maximum valid window length

---

# Approach (Sliding Window)

1. Use two pointers: `left` and `right`
2. Maintain a frequency array (size 26 for lowercase letters)
3. Keep a count of distinct characters in the current window
4. Expand `right` pointer:

   * Add current character
   * If it’s new to the window, increment distinct count
5. While distinct count > `K`:

   * Remove characters from the left
   * Decrease frequency
   * If frequency becomes 0, decrement distinct count
6. Update the maximum window length at every valid state

### Algorithm Steps

```
left = 0
maxLen = 0
for right = 0 to n-1:
    add s[right]
    if distinct > K:
        shrink window from left
    update maxLen
```

---

# Complexity

* **Time Complexity:** `O(N)`
  Each character is added and removed at most once
  
* **Space Complexity:** `O(1)`
  Fixed-size array of 26 characters

---

# Code

```java
import java.util.*;

public class Solution {
    public static int getLengthofLongestSubstring(String s, int k) {
        if (k == 0 || s.length() == 0) return 0;

        int[] freq = new int[26];
        int left = 0, distinct = 0, maxLen = 0;

        for (int right = 0; right < s.length(); right++) {
            int rChar = s.charAt(right) - 'a';
            if (freq[rChar] == 0) {
                distinct++;
            }
            freq[rChar]++;

            while (distinct > k) {
                int lChar = s.charAt(left) - 'a';
                freq[lChar]--;
                if (freq[lChar] == 0) {
                    distinct--;
                }
                left++;
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};

```

---

# Example Walkthrough

**Input**

```
S = "bacda"
K = 3
```

| Window | Substring | Distinct | Valid              |
| ------ | --------- | -------- | ------------------ |
| 0–2    | "bac"     | 3        | ✓                  |
| 1–3    | "acd"     | 3        | ✓                  |
| 1–4    | "acda"    | 3        | ✓ (max length = 4) |
| 0–4    | "bacda"   | 4        | ✗                  |

**Answer:** `4`

---

### Key Takeaway

* This is a **classic sliding window with frequency tracking**
* The moment you see “longest substring with constraints”, think:
  **Two pointers + HashMap / frequency array**