# Intuition

We’ll build **two segment trees**:
1. `evenTree`: stores the count of even numbers in each segment.
2. `oddTree`: stores the count of odd numbers in each segment.

Every update will:
* Change the value at a specific index and update both trees accordingly.

# Approach

1. **Build Segment Trees**: Recursively build `evenTree` and `oddTree`.
2. **Update Operation** (`0 x y`):
   * Check if the new value changes parity.
   * If so, update both trees at index `x`.
3. **Query Operation**:
   * `1 x y`: return `evenTree.query(x, y)`
   * `2 x y`: return `oddTree.query(x, y)`

# Complexity Analysis

* **Build Time**: O(N)
* **Each Query/Update**: O(log N)
* **Total Time**: O((N + Q) log N) → Efficient for `N, Q <= 1e5`

### **Java Code:**

```java
import java.util.*;

public class Solution {

    static class SegmentTree {
        int[] tree;
        int n;

        SegmentTree(int[] arr, boolean isEvenTree) {
            n = arr.length;
            tree = new int[4 * n];
            build(arr, 0, n - 1, 0, isEvenTree);
        }

        private void build(int[] arr, int l, int r, int idx, boolean isEvenTree) {
            if (l == r) {
                tree[idx] = (arr[l] % 2 == 0) == isEvenTree ? 1 : 0;
                return;
            }

            int mid = (l + r) / 2;
            build(arr, l, mid, 2 * idx + 1, isEvenTree);
            build(arr, mid + 1, r, 2 * idx + 2, isEvenTree);
            tree[idx] = tree[2 * idx + 1] + tree[2 * idx + 2];
        }

        public void update(int l, int r, int idx, int pos, int val, boolean isEvenTree) {
            if (l == r) {
                tree[idx] = (val % 2 == 0) == isEvenTree ? 1 : 0;
                return;
            }

            int mid = (l + r) / 2;
            if (pos <= mid) update(l, mid, 2 * idx + 1, pos, val, isEvenTree);
            else update(mid + 1, r, 2 * idx + 2, pos, val, isEvenTree);

            tree[idx] = tree[2 * idx + 1] + tree[2 * idx + 2];
        }

        public int query(int l, int r, int idx, int ql, int qr) {
            if (qr < l || ql > r) return 0;
            if (ql <= l && r <= qr) return tree[idx];

            int mid = (l + r) / 2;
            return query(l, mid, 2 * idx + 1, ql, qr) +
                   query(mid + 1, r, 2 * idx + 2, ql, qr);
        }
    }

    public static List<Integer> solve(int n, int[] arr, int q, int[][] queries) {
        SegmentTree evenTree = new SegmentTree(arr, true);
        SegmentTree oddTree = new SegmentTree(arr, false);
        List<Integer> res = new ArrayList<>();

        for (int[] query : queries) {
            int type = query[0], x = query[1], y = query[2];
            if (type == 0) {
                arr[x] = y;
                evenTree.update(0, n - 1, 0, x, y, true);
                oddTree.update(0, n - 1, 0, x, y, false);
            } else if (type == 1) {
                res.add(evenTree.query(0, n - 1, 0, x, y));
            } else {
                res.add(oddTree.query(0, n - 1, 0, x, y));
            }
        }

        return res;
    }
}
```

---
