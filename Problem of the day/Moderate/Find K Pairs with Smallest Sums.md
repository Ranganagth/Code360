# Intuition:

Brute-forcing all pairs creates `n*m` sums. Sorting them is waste. Arrays are sorted, so smallest sum pairs form a predictable frontier. For each `arr1[i]`, the smallest usable pair starts with `arr2[0]`. Next candidate grows by increasing the second index. A min-heap efficiently extracts the next smallest sum without generating everything.

---

# Approach:

Use a priority queue storing triplets `(sum, i, j)` where `i` indexes `arr1`, `j` indexes `arr2`.

1. Push `(arr1[i] + arr2[0], i, 0)` for the first `min(n,k)` elements of `arr1`.
2. Repeatedly pop the smallest sum pair.
3. Record `(arr1[i], arr2[j])`.
4. Push the next candidate `(arr1[i] + arr2[j+1], i, j+1)` if valid.
5. Stop after extracting `k` pairs.

---

# Complexity:

- **Time Complexity:** `O(k log n)` heap operations.
- **Space Complexity:** `O(n)` heap size.

---

# Code:

```java
import java.util.*;

public class Solution {
    public static ArrayList<ArrayList<Integer>> findPairs(int n, int m, ArrayList<Integer> arr1, ArrayList<Integer> arr2, int k) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<>();

        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));

        int limit = Math.min(n, k);
        for (int i = 0; i < limit; i++) {
            minHeap.offer(new int[] {arr1.get(i) + arr2.get(0), i, 0});
        }

        while (!minHeap.isEmpty() && k > 0) {
            int[] top = minHeap.poll();
            int i = top[1], j = top[2];

            ArrayList<Integer> pair = new ArrayList<>();
            pair.add(arr1.get(i));
            pair.add(arr2.get(j));
            result.add(pair);
            k--;

            if (j + 1 < m) {
                minHeap.offer(new int[] {arr1.get(i) + arr2.get(j + 1), i, j + 1});
            }
        }

        return result;
    }
};

```

---

# Example Walkthrough:

arr1 = [1,2,6], arr2 = [3,3,5], k = 3

**Initial heap entries:**
(1+3,i=0,j=0)=4
(2+3,i=1,j=0)=5
(6+3,i=2,j=0)=9

Extract 4 → pair (1,3)
Push (1+3,next j=1)=4

**Heap now:** 4,5,9

Extract 4 → pair (1,3)
Push (1+5,next j=2)=6

**Heap:** 5,6,9

Extract 5 → pair (2,3)

**Collected 3 pairs → done.**
