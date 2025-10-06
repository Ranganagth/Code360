# Intuition

We need to determine whether each pair of corresponding words in `word1` and `word2` are *identical*, where “identical” can mean either:

1. They are the same word, or
2. They are connected transitively via the given `pairs` — i.e., if `a = b` and `b = c`, then `a = c`.

This is a **connectivity** problem:

* If two words are in the same "connected group" (set), then they’re identical.

To model this, we use a **Union-Find / Disjoint Set Union (DSU)** data structure.

---

# Approach

### Step 1: Edge case

If `n != m`, immediately return `false` — the sentences must have the same number of words.

### Step 2: Union-Find setup

* Create a **Union-Find** (DSU) structure that maps each word to a "parent" (representative).
* Initially, each word is its own parent.
* For each pair `[u, v]`, **union** them (connect them).

### Step 3: Checking identical sentences

For each position `i`:

* If `word1[i]` and `word2[i]` have the **same representative** in the DSU, they are identical.
* Otherwise, return `false`.

If all words match, return `true`.

## Union-Find Helper Methods

* **find(x)** → returns representative (with path compression)
* **union(a, b)** → merges two sets

We store mappings from string → parent using a `HashMap<String, String>`.

---

# Complexity

| Type  | Complexity                   |
| ----- | ---------------------------- |
| Time  | O((n + p) * α(N)) ≈ O(n + p) |
| Space | O(p)                         |

`α(N)` is the inverse Ackermann function (very small, ~constant).

---

# Code

```java
import java.util.*;

public class Solution {

    static class UnionFind {
        private Map<String, String> parent = new HashMap<>();

        public String find(String x) {
            if (!parent.containsKey(x))
                parent.put(x, x);
            if (!parent.get(x).equals(x))
                parent.put(x, find(parent.get(x))); // Path compression
            return parent.get(x);
        }

        public void union(String a, String b) {
            String rootA = find(a);
            String rootB = find(b);
            if (!rootA.equals(rootB))
                parent.put(rootA, rootB);
        }

        public boolean isConnected(String a, String b) {
            return find(a).equals(find(b));
        }
    }

    public static boolean identicalSentences(ArrayList<String> word1, ArrayList<String> word2,
                                             ArrayList<ArrayList<String>> pairs, int n, int m, int p) {

        // If lengths differ, they can never be identical
        if (n != m) return false;

        UnionFind uf = new UnionFind();

        // Build connections
        for (ArrayList<String> pair : pairs) {
            uf.union(pair.get(0), pair.get(1));
        }

        // Check if each corresponding word is connected
        for (int i = 0; i < n; i++) {
            String w1 = word1.get(i);
            String w2 = word2.get(i);
            if (!w1.equals(w2) && !uf.isConnected(w1, w2)) {
                return false;
            }
        }

        return true;
    }
};

```

---

## Example Walkthrough

**Input:**

```
word1 = ["better", "life"]
word2 = ["fine", "country"]
pairs = [["better", "good"], ["fine", "great"], ["great", "good"], ["life", "fine"]]
```

**Union steps:**

* better ↔ good
* fine ↔ great
* great ↔ good  → now better, good, great, fine are all connected
* life ↔ fine  → now life also joins that group

Groups:

```
{better, good, great, fine, life}
```

Check:

* better ↔ fine → (connected)
* life ↔ country → (not connected)

**Result:** `False`

## Example Verification

**Input:**

```
word1 = ["this", "is", "it"]
word2 = ["that", "is", "it"]
pairs = [["that", "this"]]
```

Union: `that ↔ this`
Checks:

* this ↔ that → true
* is ↔ is → true
* it ↔ it → true

Output: `True`

---
