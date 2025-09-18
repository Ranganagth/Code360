# Intuition

We need the `k`-th smallest element in an unsorted array.
Possible approaches:

1. **Sort and pick:** Sort the array, return element at index `k-1`.

   * Simple, but `O(N log N)`.
2. **Heap approach (better for large N, small K):**

   * Maintain a **max-heap** of size `k`.
   * Traverse array, push elements into heap.
   * If heap size exceeds `k`, pop the largest.
   * At the end, heap top = `k`-th smallest element.
   * Complexity: `O(N log K)`, space `O(K)`.
3. **Quickselect (average O(N)):** More complex but efficient. Not required unless problem constraints are extreme.

Since `N ≤ 10^5` and `K ≤ N`, **Heap solution or Sort solution works fine**.

---

# Approach (Heap)

1. Create a **max-heap** (PriorityQueue with reverse comparator).
2. Traverse array:

   * Add element to heap.
   * If size > k, remove top (largest).
3. After traversal, heap top = answer.

---

# Complexity

* **Time:** `O(N log K)` (much faster than full sorting when `K` is small).
* **Space:** `O(K)`.

---
# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int kthSmallest(ArrayList<Integer> v, int n, int k) {
        // Max-heap of size k
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        
        for (int num : v) {
            maxHeap.add(num);
            if (maxHeap.size() > k) {
                maxHeap.poll(); // remove largest
            }
        }
        
        return maxHeap.peek(); // top is kth smallest
    }
}
```

---
## Example Walkthrough

Input: `ARR = [5, 2, 1, 4, 3, 6], K = 3`

* Insert 5 → heap = \[5]
* Insert 2 → heap = \[5, 2]
* Insert 1 → heap = \[5, 2, 1]
* Insert 4 → heap = \[5, 4, 1, 2] → pop 5 → heap = \[4, 2, 1]
* Insert 3 → heap = \[4, 3, 1, 2] → pop 4 → heap = \[3, 2, 1]
* Insert 6 → heap = \[6, 3, 1, 2] → pop 6 → heap = \[3, 2, 1]

Answer = top of heap = **3**

---
