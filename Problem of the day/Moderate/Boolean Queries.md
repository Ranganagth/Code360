# Intuition

1. **Naive approach**: Maintain a boolean array of size `10^5` and update values directly.

   * Problem: If we call `setAllTrue()` or `setAllFalse()`, we’d need to update `10^5` elements each time → `O(N)` per query, which is too slow.

2. **Optimized approach**:

   * Instead of updating all elements, maintain a **global state** representing what all elements should be if not overridden.
   * Keep a `Map<Integer, Boolean>` (or array) for **overrides** where specific indices differ from the global state.
   * On `setAllTrue` or `setAllFalse`, just update the **global state** and **clear overrides**.
   * On `setIndexTrue` / `setIndexFalse`, mark the override.
   * On `getIndex`, return the override if present, else the global state.

This reduces:

* Bulk operation (`setAllTrue`, `setAllFalse`) → `O(1)`
* Point updates (`setIndexTrue`, `setIndexFalse`) → `O(1)`
* Queries (`getIndex`) → `O(1)`

---

# Approach

We maintain:

* `boolean globalValue` → default value of all indices.
* `Map<Integer, Boolean> overrides` → stores only indices that differ from `globalValue`.

### Operations

* `setAllTrue()` → `globalValue = true; overrides.clear();`
* `setAllFalse()` → `globalValue = false; overrides.clear();`
* `setIndexTrue(i)`:

  * If `globalValue == true` → remove override if exists (no need to store).
  * Else put `overrides[i] = true`.
* `setIndexFalse(i)`:

  * If `globalValue == false` → remove override if exists.
  * Else put `overrides[i] = false`.
* `getIndex(i)`:

  * If in overrides → return overrides\[i].
  * Else return `globalValue`.

---

# Complexity

* Time per query: **O(1)**
* Space: **O(Q)** in worst case (if many overrides stored).

---

# Code

```java
import java.util.*;

public class InfiniteArray {

    private boolean globalValue; // default value for all indices
    private Map<Integer, Boolean> overrides;

    InfiniteArray() {
        this.globalValue = false; // initially all are false
        this.overrides = new HashMap<>();
    }

    void setAllTrue() {
        globalValue = true;
        overrides.clear();
    }

    void setAllFalse() {
        globalValue = false;
        overrides.clear();
    }

    void setIndexTrue(int index) {
        if (globalValue) {
            overrides.remove(index); // already true globally
        } else {
            overrides.put(index, true);
        }
    }

    void setIndexFalse(int index) {
        if (!globalValue) {
            overrides.remove(index); // already false globally
        } else {
            overrides.put(index, false);
        }
    }

    boolean getIndex(int index) {
        return overrides.getOrDefault(index, globalValue);
    }
}
```

---

## Example Walkthrough

### Input

```
3 queries:
1 2 1
3 0
2 4
```

1. `1 2 1` → set index 2 = true

   * `globalValue = false`
   * `overrides = {2 -> true}`

2. `3 0` → set all false

   * `globalValue = false`
   * `overrides.clear()`

3. `2 4` → get index 4

   * Not in overrides → return `globalValue = false`
     → Output: `False`

---

