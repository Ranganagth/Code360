# Intuition

A word is valid if it can be formed by **concatenating ≥ 2 smaller words from the same list**.

This is a **word break problem with constraint ≥ 2 parts**.

Use:

* `HashSet` for fast lookup
* For each word → check if it can be formed using DP

---

# Approach

1. Insert all words into a set
2. For each word:

   * temporarily remove it from set (to avoid using itself)
   * apply DP (word break)
   * ensure at least **2 parts used**
3. Add valid words to result

## DP Logic

$$dp[i] = true → substring [0..i) can be formed
$$

---

# Complexity

* **Time complexity:**
$$O(N * L^2)
$$
* **Space complexity:**
$$O(L)
$$

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

	public static String[] getConcatenatedWords(int n, String[] words) {

		HashSet<String> set = new HashSet<>(Arrays.asList(words));
		ArrayList<String> result = new ArrayList<>();

		for (String word : words) {

			set.remove(word);

			if (canForm(word, set)) {
				result.add(word);
			}

			set.add(word);
		}

		return result.toArray(new String[0]);
	}

	private static boolean canForm(String word, HashSet<String> set) {

		int len = word.length();
		boolean[] dp = new boolean[len + 1];
		dp[0] = true;

		for (int i = 1; i <= len; i++) {

			for (int j = 0; j < i; j++) {

				if (!dp[j]) continue;

				if (set.contains(word.substring(j, i))) {
					dp[i] = true;
					break;
				}
			}
		}

		return dp[len];
	}
};

```

---

# Example

**Input:**
["cat", "dog", "catdog"]

Check "catdog":
→ "cat" + "dog" → valid

**Output:** ["catdog"]


---

# Key Insight

- Classic Word Break + remove current word to avoid self usage
