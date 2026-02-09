# Intuition

The string contains words separated by spaces.
The task is to count how many times each query word appears in the string while preserving the order of the query list.
Efficient counting requires preprocessing the string once, instead of scanning the string separately for each query.

---

# Approach

1. Split the input string using whitespace into tokens (words).
2. Build a frequency map (`HashMap<String,Integer>`) counting occurrences of each token.
3. For every word in the query list:
   * Look up its count in the map.
   * If absent, return 0.
4. Store counts in the same order as the query list.

This ensures only one pass over the string.

---

# Complexity

* Splitting string: `O(|STR|)`
* Building frequency map: `O(number_of_words)`
* Query lookup: `O(N)`
* **Total Time:** `O(|STR| + N)`
* **Space:** `O(unique_words_in_STR)`

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static ArrayList<Integer> getFrequency(String str, ArrayList<String> words, int n) {

        ArrayList<Integer> result = new ArrayList<>();

        // Step 1: tokenize string
        String[] tokens = str.split("\\s+");

        // Step 2: build frequency map
        HashMap<String, Integer> freq = new HashMap<>();
        for (String t : tokens) {
            freq.put(t, freq.getOrDefault(t, 0) + 1);
        }

        // Step 3: answer queries
        for (String w : words) {
            result.add(freq.getOrDefault(w, 0));
        }

        return result;
    }
};

```

---

# Example Walkthrough

**Input:**

```
STR = "i am a Ninja"
WORDS = ["Ninja","a","am"]
```

Step-1 tokens:

```
["i","am","a","Ninja"]
```

Step-2 frequency map:

```
i → 1
am → 1
a → 1
Ninja → 1
```

Step-3 query lookup:

```
"Ninja" → 1
"a"     → 1
"am"    → 1
```

**Output:**

```
[1, 1, 1]
```

---
