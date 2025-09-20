# Intuition

* We want the array to be as lexicographically small as possible.
* That means: for every position `i`, we try to bring the **smallest possible element within reach (`i..i+k`)** to position `i`.
* To move an element from index `j` to index `i`, it costs `(j - i)` swaps (each adjacent swap shifts it left by 1).
* If `(j - i) <= k`, we can bring that element to position `i` by repeatedly swapping left.
* After placing it, reduce `k` by `(j - i)`.

This greedy strategy works because lexicographic order is determined by the earliest differing index, so we must always try to minimize from the leftmost side.

---

# Approach

1. Loop over each index `i` from `0..n-1`.
2. Look ahead up to `i + k` (or array end) and find the smallest element.
3. Bring that element to index `i` by shifting elements between.

   * Decrease `k` by the distance moved.
4. Stop early if `k` runs out.

---

# Complexity

* Worst-case: For each index, we may scan up to `k` range.
* But since we also shift elements, complexity is **O(N²)** worst-case.
* With constraints `N ≤ 5000`, `O(N²)` is acceptable.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int[] findSmallestWithKSwaps(int arr[], int n, int k) {
        for (int i = 0; i < n - 1 && k > 0; i++) {
            // Find the smallest element in arr[i..i+k]
            int pos = i;
            for (int j = i + 1; j < n && j - i <= k; j++) {
                if (arr[j] < arr[pos]) {
                    pos = j;
                }
            }

            // Bring arr[pos] to position i by shifting left
            for (int j = pos; j > i; j--) {
                int temp = arr[j];
                arr[j] = arr[j - 1];
                arr[j - 1] = temp;
            }

            // Update remaining k
            k -= (pos - i);
        }
        return arr;
    }
}
```

---
## Example Walkthrough

Input:

```
ARR = [3, 5, 1, 4], K = 2
```

* i=0: Look ahead up to index `2`. Smallest = `1` at index `2`.

  * Distance = 2, within K.
  * Bring `1` forward: \[1, 3, 5, 4], K = 0.
* Done (K exhausted).
* Output = \[1, 3, 5, 4].

Correct.
### Sample Run

Input:

```
4 2
3 5 1 4
```

Output:

```
1 3 5 4
```

Input:

```
5 5
1 3 5 7 9
```

Output:

```
1 3 5 7 9
```

---
