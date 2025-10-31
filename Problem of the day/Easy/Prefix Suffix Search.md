# Intuition

We are asked to design a data structure **PRE_SUF_SEARCH** that can:

1. Store a list of words.
2. Efficiently answer queries of the form:

   ```
   find(prefix, suffix)
   ```

   returning the **largest index** of the word that **starts with `prefix` and ends with `suffix`**.

## Brute Force Idea

For each query `(prefix, suffix)`:

* Loop through all words in reverse order (to get the *largest index first*).
* For each word:

  * Check if it **startsWith(prefix)** and **endsWith(suffix)**.
  * Return the first match.
* If no match is found → return -1.

This approach is **simple** and works efficiently for moderate constraints (since `|word| <= 10`, and `N, Q <= 10^4`).

---

# Optimized Approach (Prefix–Suffix Map idea)

If we wanted even faster lookups, we could precompute a map:

```
key = prefix + "#" + suffix → value = largest index
```

But this requires generating all possible prefix–suffix combinations for each word, which can be `O(L^2)` per word.
Given `L <= 10`, this is fine — but implementing it directly can be heavy.

So we’ll show both options below.

---

# Approach 1 (Simple + Efficient Enough for Given Constraints)

1. Store words globally in a list.
2. When `find(prefix, suffix)` is called:

   * Iterate from **end to start**.
   * Check if a word **startsWith(prefix)** and **endsWith(suffix)**.
   * Return its index immediately when found.
3. If no word matches, return -1.

---

## Example Walkthrough

### Input:

```
WORDS = ["cat", "bat", "rat"]
Queries:
1) prefix = "ca", suffix = "t"
2) prefix = "b",  suffix = "t"
3) prefix = "rr", suffix = "t"
```

**Step-by-step:**

1. Query 1:

   * "rat" → no
   * "bat" → no
   * "cat" → yes → return index 0
2. Query 2:

   * "rat" → no
   * "bat" → yes → return index 1
3. Query 3:

   * none match → return -1

**Output:**

```
0
1
-1
```

---

# Time Complexity

* `wordFilter`: O(N)
* `find`: O(N * L) per query (where L ≤ 10)
* Acceptable since N, Q ≤ 10^4 and L is small.

**Space Complexity:** O(N)

---

# Code

```java
import java.util.*;

public class Solution {
    private static String[] wordsList;

    // Store all words in the PRE_SUF_SEARCH structure
    public static void wordFilter(String[] words) {
        wordsList = words;
    }

    // Find the word with given prefix and suffix having largest index
    public static int find(String prefix, String suffix) {
        // Start from the end to get the largest index
        for (int i = wordsList.length - 1; i >= 0; i--) {
            String word = wordsList[i];
            if (word.startsWith(prefix) && word.endsWith(suffix)) {
                return i;
            }
        }
        return -1; // No match found
    }
}
```

---

## Approach 2 (Optimized Using Precomputed Prefix–Suffix Map)

If you want **O(1)** query time (but more preprocessing):

```java
import java.util.*;

public class Solution {
    private static Map<String, Integer> map = new HashMap<>();

    public static void wordFilter(String[] words) {
        map.clear();
        for (int index = 0; index < words.length; index++) {
            String word = words[index];
            int len = word.length();
            for (int i = 1; i <= len; i++) {
                String prefix = word.substring(0, i);
                for (int j = 0; j < len; j++) {
                    String suffix = word.substring(j);
                    String key = prefix + "#" + suffix;
                    map.put(key, index); // Store largest index
                }
            }
        }
    }

    public static int find(String prefix, String suffix) {
        String key = prefix + "#" + suffix;
        return map.getOrDefault(key, -1);
    }
}
```

### Trade-off:

* Preprocessing: O(N * L²)
* Query time: O(1)
* Ideal when many queries (`Q`) compared to words (`N`).

---

## Example Output

### Input

```
WORDS = ["cat", "bat", "rat"]
Queries:
ca t
b t
rr t
```

### Output

```
0
1
-1
```

---

