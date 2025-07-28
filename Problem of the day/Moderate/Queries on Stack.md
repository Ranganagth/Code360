# Intuition

The problem mimics operations on a constrained stack with an upper limit, supporting three custom operations: `PUSH`, `POP`, and `INC`. We need to simulate these operations while ensuring correctness and efficiency—especially for the `INC` operation, which must increment bottom `K` elements without unnecessary iterations.

---

# Approach

We will use:
* A stack (`ArrayDeque`) to simulate standard push and pop operations.
* An auxiliary array `inc[]` to lazily propagate the increment operations, avoiding repetitive updates of bottom elements during each `INC`.

## Key Observations:

* When `INC k y` is encountered, we store this increment in `inc[k-1] += y`. This means: when we eventually pop element at index `k-1`, we add this pending increment.
* When popping, we apply and propagate the increment to the next lower element to ensure proper chaining.

## Steps:

1. Initialize a `stack` to hold values and an `inc[]` list of same size to track incremental values.
2. For each query:
   * If `PUSH x`: Push only if current size < `limit`. Append `x` and 0 (for `inc`) to respective lists.
   * If `POP`: If empty, return `-1`. Else, apply the top `inc`, and propagate to next below element.
   * If `INC k y`: Add `y` to `inc[min(k, size)-1]`.

---

# Complexity

* **Time complexity**:
  * Each operation is `O(1)`, except worst-case `INC` on a naive approach would be `O(k)`.
  * Using the lazy increment technique (with propagation on `POP`), we ensure all operations run in `O(1)` amortized time.

* **Space complexity**:
  * `O(limit)` for the stack and auxiliary increment array.
  * `O(Q)` for storing result of all `POP` operations.

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> answerQueries(ArrayList<ArrayList<Integer>> queries, int limit) {
        ArrayDeque<Integer> stack = new ArrayDeque<>();
        ArrayList<Integer> inc = new ArrayList<>();
        ArrayList<Integer> result = new ArrayList<>();

        for (ArrayList<Integer> query : queries) {
            int type = query.get(0);

            if (type == 1) { // PUSH X
                int x = query.get(1);
                if (stack.size() < limit) {
                    stack.push(x);
                    inc.add(0); // corresponding increment
                }
            } else if (type == 2) { // POP
                if (stack.isEmpty()) {
                    result.add(-1);
                } else {
                    int topIndex = stack.size() - 1;
                    int addVal = inc.get(topIndex);
                    int val = stack.pop() + addVal;
                    inc.remove(topIndex);
                    if (!inc.isEmpty()) {
                        int prev = inc.get(topIndex - 1);
                        inc.set(topIndex - 1, prev + addVal);
                    }
                    result.add(val);
                }
            } else if (type == 3) { // INC K Y
                int k = query.get(1);
                int y = query.get(2);
                if (!inc.isEmpty()) {
                    int idx = Math.min(k, inc.size()) - 1;
                    inc.set(idx, inc.get(idx) + y);
                }
            }
        }
        return result;
    }
}
```
