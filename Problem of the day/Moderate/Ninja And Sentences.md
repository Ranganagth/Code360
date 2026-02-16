# Intuition

We are given multiple lists of words. The task is to generate all possible sentences by picking **one word from each list in order**.
This is essentially a **cartesian product** of the given lists.

---

# Approach

1. **Start with an empty sentence** in a queue.
2. For each row (list of words):

   * Expand all current partial sentences by appending each word from this row.
   * Push these new sentences into the queue.
3. After processing all rows, the queue contains all valid sentences.
4. Copy the results to the answer list.

---

# Complexity

* **Time Complexity:**
  If there are `r` rows and each row has at most `c` words, the total combinations are
  `c^r`.
  Thus, time complexity = **O(c^r \* r)** (since each sentence has length `r`).
* **Space Complexity:**
  We need to store all results = **O(c^r \* r)**.

---

# Code

```java
import java.util.*;

public class Solution {
    public static void createSentences(ArrayList<ArrayList<String>> arr,
                                       ArrayList<ArrayList<String>> ans) {
        Queue<ArrayList<String>> queue = new LinkedList<>();

        // Start with an empty sentence
        queue.add(new ArrayList<>());

        for (ArrayList<String> row : arr) {
            int size = queue.size();

            // Expand each current sentence
            while (size-- > 0) {
                ArrayList<String> current = queue.poll();
                for (String word : row) {
                    ArrayList<String> newSentence = new ArrayList<>(current);
                    newSentence.add(word);
                    queue.add(newSentence);
                }
            }
        }

        ans.addAll(queue);
    }

    // Small driver to test
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt(); // test cases
        while (t-- > 0) {
            int r = sc.nextInt(), c = sc.nextInt();
            ArrayList<ArrayList<String>> arr = new ArrayList<>();
            for (int i = 0; i < r; i++) {
                ArrayList<String> row = new ArrayList<>();
                for (int j = 0; j < c; j++) {
                    row.add(sc.next());
                }
                arr.add(row);
            }

            ArrayList<ArrayList<String>> ans = new ArrayList<>();
            createSentences(arr, ans);

            for (ArrayList<String> sentence : ans) {
                System.out.println(String.join(" ", sentence));
            }
        }
    }
}
```

---

# Example walkthrough with explanation

**Input:**

```
1
2 2
you we
sleep drink
```

* Start with `[]`
* First row: `["you", "we"]` → sentences become:
  `[you]`, `[we]`
* Second row: `["sleep", "drink"]` → expand each:

  * `[you]` → `[you sleep]`, `[you drink]`
  * `[we]` → `[we sleep]`, `[we drink]`

**Output:**

```
you sleep
you drink
we sleep
we drink
```

---
