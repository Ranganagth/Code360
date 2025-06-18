# Intuition(BFS-Based)

* Start with `"1"` in the queue.
* For every element in the queue:

  * Add it to the result.
  * Push current + `'0'` and current + `'1'` to the queue.
  * Do this until you generate `N` binary numbers.

This is much faster than converting each number to binary one by one, especially when `N` is large (`up to 10^5`).

---
# Complexity Analysis

* **Time Complexity:** O(N) — We generate exactly `N` binary numbers.
* **Space Complexity:** O(N) — For queue and result list.

---
# Code:

```java
import java.util.*;

public class Solution {

    public static List<String> generateBinaryNumbers(int n) {
        List<String> result = new ArrayList<>();
        Queue<String> queue = new LinkedList<>();
        
        queue.offer("1");
        
        for (int i = 0; i < n; i++) {
            String current = queue.poll();
            result.add(current);
            
            // Generate next binary numbers
            queue.offer(current + "0");
            queue.offer(current + "1");
        }
        
        return result;
    }
}
```

---

### Example:

Input:

```
N = 6
```

Output:

```
["1", "10", "11", "100", "101", "110"]
```

---

