# Intuition

To solve the problem efficiently, we can **flatten the nested list lazily** using a **stack-based approach**. This ensures we only process what we need, and we avoid flattening the entire structure upfront (which could be costly for large inputs).

---

# Approach

* Use a **stack** to simulate the recursive flattening process.
* Each stack entry holds an **iterator** over a list of `NestedInteger`.
* When `hasNext()` is called, we:

  * Traverse until we find an integer at the top of the stack.
  * If a list is found, push its iterator onto the stack.
* The `next()` method simply returns the integer stored by `hasNext()`.

---

# Code

```java
import java.util.*;

public class NestedIterator {
    private Stack<Iterator<NestedInteger>> stack;
    private Integer nextInteger;

    public NestedIterator(List<NestedInteger> nestedList) {
        stack = new Stack<>();
        stack.push(nestedList.iterator());
    }

    public Integer next() {
        return nextInteger;
    }

    public boolean hasNext() {
        while (!stack.isEmpty()) {
            Iterator<NestedInteger> current = stack.peek();
            if (!current.hasNext()) {
                stack.pop();
                continue;
            }

            NestedInteger ni = current.next();
            if (ni.isInteger()) {
                nextInteger = ni.getInteger();
                return true;
            } else {
                stack.push(ni.getList().iterator());
            }
        }
        return false;
    }
}
```

---

# Complexity:

* **Time complexity**:

  * `next()` and `hasNext()` both run in **amortized O(1)** time over the total number of integers.
* **Space complexity**:

  * At most **O(D)** space where `D` is the depth of the nested list (for the stack).

---

### **Example Walkthrough**:

Input: `[[1,1],2,[1,1]]`

Stack progresses as:

1. Push iterator of entire list.
2. `hasNext()` sees `[1,1]` → pushes its iterator.
3. Retrieves 1 and 1.
4. Retrieves 2.
5. Sees `[1,1]` again → pushes its iterator and retrieves 1 and 1.

Final flattened output: `[1,1,2,1,1]`
