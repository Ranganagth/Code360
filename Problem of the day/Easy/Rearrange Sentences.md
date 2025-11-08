# Intuition

We are asked to reorder a list of sentences based on whether their content (after the identifier) is **letters** or **numbers**.

* **Letter sentences** (words made of lowercase letters) must come **before** number sentences.
* Among letter sentences, sorting is **lexicographical based on the content after the identifier**, and the identifier is used only if there’s a tie.
* **Number sentences** should retain their **original order**.

This is similar to the “log files reorder” problem — we essentially partition logs into two groups and sort one of them while keeping the other stable.

---

# Approach

1. **Separate the sentences** into two lists:

   * **letterSentences**: sentences where the first word after identifier is a lowercase word.
   * **numberSentences**: sentences where the first word after identifier is a number.

2. **Custom Sorting for letterSentences**:

   * Compare based on substring after the identifier (the “content”).
   * If the content is the same, use the identifier to break ties.

   Example of sorting key:
   `"a1 coding ninjas"` → key = ("coding ninjas", "a1")

3. **Keep numberSentences in input order** (no sorting).

4. **Concatenate** sorted letterSentences and original numberSentences.

---

# Complexity

| Operation                | Time       | Space |
| ------------------------ | ---------- | ----- |
| Splitting sentences      | O(N)       | O(N)  |
| Sorting letter sentences | O(L log L) | O(L)  |
| Concatenation            | O(N)       | O(1)  |

Where `L` = number of letter sentences, `N` = total sentences.

**Overall:**
Time Complexity → `O(N log N)`
Space Complexity → `O(N)`

---

# Code

```java
import java.util.*;

public class Solution {
    public static List<String> reOrderSentences(String[] sentences) {
        List<String> letterSentences = new ArrayList<>();
        List<String> numberSentences = new ArrayList<>();

        for (String sentence : sentences) {
            String[] parts = sentence.split(" ", 2);
            if (Character.isDigit(parts[1].charAt(0))) {
                numberSentences.add(sentence);
            } else {
                letterSentences.add(sentence);
            }
        }

        Collections.sort(letterSentences, (s1, s2) -> {
            String[] p1 = s1.split(" ", 2);
            String[] p2 = s2.split(" ", 2);

            int cmp = p1[1].compareTo(p2[1]);
            if (cmp == 0) {
                return p1[0].compareTo(p2[0]);
            }
            return cmp;
        });

        List<String> result = new ArrayList<>();
        result.addAll(letterSentences);
        result.addAll(numberSentences);

        return result;
    }
};

```


---

## Example Walkthrough

**Input:**

```
3
d1 2 3
love8 coding world
a1 coding ninjas
```

**Step 1:** Classify

* Letter Sentences: `"love8 coding world"`, `"a1 coding ninjas"`
* Number Sentences: `"d1 2 3"`

**Step 2:** Sort letter sentences

* Keys:

  * `"love8 coding world"` → ("coding world", "love8")
  * `"a1 coding ninjas"` → ("coding ninjas", "a1")
* Sorted order → `"a1 coding ninjas"`, `"love8 coding world"`

**Step 3:** Combine
Final order →

```
a1 coding ninjas
love8 coding world
d1 2 3
```


---

## Verification with Sample Input

**Input:**

```
3
d1 2 3
love8 coding world
a1 coding ninjas
```

**Output:**

```
a1 coding ninjas
love8 coding world
d1 2 3
```

**Works correctly.**

---
## Summary

This approach ensures:

* Lexicographical order among letter sentences.
* Stability for number sentences.
* Efficient time and space usage.
