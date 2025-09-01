# Intuition

The problem asks for the maximum value of `A xor B` where `A` is from `arr1` and `B` is from `arr2`.
A naive solution would try all pairs `(A, B)`, but that would take `O(N*M)` time, which is too slow for large arrays.

We can do better using a **bitwise Trie (prefix tree)**.

* A Trie helps us quickly find, for a given number, another number that maximizes its XOR.
* The key idea is: to maximize XOR, we should try to match opposite bits as we traverse from the most significant bit (MSB) to the least significant bit (LSB).

So, if we insert all elements of one array into a Trie, we can query each element of the other array in `O(31)` (since max 10^9 < 2^30).

---

# Approach

1. **Build a Trie:**

   * Insert all numbers of `arr2` into a binary Trie where each node represents a `0` or `1` bit.
   * Each number has up to 31 bits (since `10^9 < 2^30`).

2. **Query each number in arr1:**

   * For each number in `arr1`, traverse the Trie from MSB to LSB.
   * At each bit, we greedily choose the opposite bit if it exists in the Trie, because that maximizes XOR.
   * Collect the result bit by bit to compute maximum possible XOR with numbers in `arr2`.

3. **Take maximum:**

   * Track the maximum value across all queries.

This reduces complexity significantly while ensuring correctness.

---

# Complexity

* **Time Complexity:**

  * Building the Trie: `O(M * 31)`
  * Querying for each number in `arr1`: `O(N * 31)`
  * Total: `O((N + M) * 31)` ≈ `O(N + M)`
* **Space Complexity:**

  * Trie storage: `O(M * 31)` ≈ `O(M)`

---

# Code

```java
import java.util.ArrayList;

class TrieNode {
    TrieNode[] children = new TrieNode[2];
}

public class Solution {

    private static void insert(TrieNode root, int num) {
        TrieNode node = root;
        for (int i = 30; i >= 0; i--) { // max 31 bits for 10^9
            int bit = (num >> i) & 1;
            if (node.children[bit] == null) {
                node.children[bit] = new TrieNode();
            }
            node = node.children[bit];
        }
    }

    private static int query(TrieNode root, int num) {
        TrieNode node = root;
        int xorVal = 0;
        for (int i = 30; i >= 0; i--) {
            int bit = (num >> i) & 1;
            int opposite = 1 - bit;
            if (node.children[opposite] != null) {
                xorVal |= (1 << i); // we can set this bit in result
                node = node.children[opposite];
            } else {
                node = node.children[bit];
            }
        }
        return xorVal;
    }

    public static int maxXOR(int n, int m, ArrayList<Integer> arr1, ArrayList<Integer> arr2) {
        TrieNode root = new TrieNode();

        // Insert all elements of arr2 into Trie
        for (int num : arr2) {
            insert(root, num);
        }

        int maxResult = 0;
        // Query each element of arr1
        for (int num : arr1) {
            maxResult = Math.max(maxResult, query(root, num));
        }

        return maxResult;
    }
}
```

---

## Example Walkthrough

### Input:

```
arr1 = [6, 6, 0, 6, 8, 5, 6]
arr2 = [1, 7, 1, 7, 8, 0, 2]
```

### Step 1: Insert arr2 into Trie

* Numbers like `1, 7, 8, 2` are inserted bit by bit.

### Step 2: Query arr1 elements

* Take `8 (1000 in binary)`:

  * Check in Trie, we can match with `7 (0111)` to maximize XOR.
  * `8 xor 7 = 15`.
* Take `6 (0110)`:

  * Best match found is `9` equivalent path → XOR = 13.
* Similarly for other numbers.

### Step 3: Maximum

* The maximum value found is **15**, which is the answer.

---
