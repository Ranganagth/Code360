### Problem understanding

To solve this problem, we need to check whether the third string (`THIRD`) contains **all** the characters from the first and second strings (`FIRST` and `SECOND`) **with their exact counts**. This means we have to check **frequency** of each character.

---

# Intuition

* Combine `FIRST` and `SECOND` into a single frequency map.
* Create a frequency map of `THIRD`.
* Compare both frequency maps. If `THIRD` has **at least as many** of each character as required by the combined map, return `"YES"`, else `"NO"`.

---

# Approach

1. Count characters in `FIRST + SECOND`.
2. Count characters in `THIRD`.
3. For each character in the first frequency map, ensure `third` does **not** have any **extra** characters beyond what's in `first + second`.
4. Repeat for all test cases.

---

# Time Complexity

* For each test case:
  * Counting characters: `O(n + m + p)` where n = `|FIRST|`, m = `|SECOND|`, p = `|THIRD|`
  
* Total across T test cases: `O(T * maxLength)`

---

# Java

```java
import java.util.*;

public class Solution {
	public static String amazingStrings(String first, String second, String third) {
		int[] required = new int[26];
		int[] actual = new int[26];

		// Count characters in first and second
		for (char c : first.toCharArray()) {
			required[c - 'A']++;
		}
		for (char c : second.toCharArray()) {
			required[c - 'A']++;
		}

		// Count characters in third
		for (char c : third.toCharArray()) {
			actual[c - 'A']++;
		}

		// Compare frequencies
		for (int i = 0; i < 26; i++) {
			if (actual[i] != required[i]) {
				return "NO";
			}
		}
		return "YES";
	}
}
```

---

### **Example Walkthrough**

**Test Case 1:**

```
FIRST: HI
SECOND: HEY
THIRD: EIHYH
```

* Combined required: H:2, I:1, E:1, Y:1
* Third has: E:1, I:1, H:2, Y:1 → YES

**Test Case 2:**

```
FIRST: ALL
SECOND: GOOD
THIRD: ADOLLG
```

* Combined required: A:1, L:2, G:1, O:2, D:1
* Third has: A:1, D:1, O:1, L:2, G:1 → only 1 O < required 2 → NO

---

### Input Format

```
T
FIRST1 SECOND1 THIRD1
FIRST2 SECOND2 THIRD2
...
```

### Output Format

One line per test case:

```
YES
NO
...
```

---
