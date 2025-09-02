# Intuition

We need to find **all occurrences** of each string in `ARR` inside a given string `S`.

* For each word in `ARR`, we should scan through `S` and check where it appears as a substring.
* The simplest way: **slide a window of length equal to the word’s length** over `S`, and compare substrings.

This is fine because constraints are small (`|S| <= 100`, `|ARR[i]| <= 100`), so even a brute-force sliding approach is acceptable within the time limit.

---

# Approach

1. For each query word `word` in `ARR`:

   * Let `m = word.length()`.
   * Iterate over all starting indices `i` from `0` to `S.length() - m`.
   * Check if `S.substring(i, i+m).equals(word)`.

     * If true, store this index.
   * Collect all such indices.
2. Return all the collected indices for each query word in order.

---

# Complexity Analysis

* For each query word of length `m`, we slide across `S` of length `n`.
* Each substring comparison takes `O(m)` in worst case.
* Worst-case per word: `O(n*m)`.
* For `N` query words: `O(N * n * m)`.

Since `n, m, N ≤ 100`, the max work is `100 * 100 * 100 = 10^6`, which is efficient under 1 sec.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int[][] findOccurrences(String S, String[] arr) {
        int n = arr.length;
        List<List<Integer>> result = new ArrayList<>();
        
        for (String word : arr) {
            List<Integer> indices = new ArrayList<>();
            int m = word.length();
            
            for (int i = 0; i + m <= S.length(); i++) {
                if (S.substring(i, i + m).equals(word)) {
                    indices.add(i);
                }
            }
            
            result.add(indices);
        }
        
        // Convert List<List<Integer>> → int[][]
        int[][] ans = new int[n][];
        for (int i = 0; i < n; i++) {
            ans[i] = result.get(i).stream().mapToInt(Integer::intValue).toArray();
        }
        return ans;
    }

    // Helper for testing
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();
        
        while (T-- > 0) {
            int N = sc.nextInt();
            String[] arr = new String[N];
            for (int i = 0; i < N; i++) arr[i] = sc.next();
            String S = sc.next();
            
            int[][] res = findOccurrences(S, arr);
            for (int[] row : res) {
                if (row.length == 0) {
                    System.out.println();
                } else {
                    for (int idx : row) {
                        System.out.print(idx + " ");
                    }
                    System.out.println();
                }
            }
        }
    }
}
```

---

## **Example Walkthrough**

Input:

```
ARR = ["aa", "ab", "b"]
S = "abaab"
```

* Word `"aa"` (length 2):

  * Check S\[0..2) = "ab" → no
  * Check S\[1..3) = "ba" → no
  * Check S\[2..4) = "aa" → match → index 2
  * Check S\[3..5) = "ab" → no
    → Occurrences = `[2]`

* Word `"ab"` (length 2):

  * S\[0..2) = "ab" → match → index 0
  * S\[1..3) = "ba" → no
  * S\[2..4) = "aa" → no
  * S\[3..5) = "ab" → match → index 3
    → Occurrences = `[0, 3]`

* Word `"b"` (length 1):

  * S\[0] = "a" → no
  * S\[1] = "b" → match → index 1
  * S\[2] = "a" → no
  * S\[3] = "a" → no
  * S\[4] = "b" → match → index 4
    → Occurrences = `[1, 4]`

Output:

```
2
0 3
1 4
```

---
