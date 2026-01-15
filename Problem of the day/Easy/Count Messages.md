# Intuition

This is an **offline message logging + range aggregation** problem.
Type-1 queries only append data.
Type-2 queries ask for **frequency counts over fixed-size time buckets** for a specific user.

**Constraints are small:**

* Total queries ≤ 100
* Time ≤ 3000
* Users short strings

So the correct approach is **store raw timestamps per user**, then compute counts on demand.

No preprocessing, no compression, no advanced indexing required.

---

# Approach

**Data structure**
* Maintain a global map
  `Map<String, List<Integer>> userMessages`
* For every user, store **all message timestamps** received so far

**Handling queries**

1. receiveMessage(user, time)
	* Append `time` to the list for `user`
	* If user not present, create a new list

2. getMessageCount(l, r, user, k)

**Steps:**

* Compute number of chunks:

  ```
  chunks = ceil((r - l + 1) / k)
  ```
* Initialize an array `count[chunks]` with zeros
* If user does not exist → return all zeros
* For each timestamp `t` of that user:

  * If `t < l` or `t > r`, ignore
  * Compute chunk index:

    ```
    index = (t - l) / k
    ```
  * Increment `count[index]`
* Return the count array as a list

**Chunk definition is fixed and contiguous:**

* Chunk 0 → [l, l+k-1]
* Chunk 1 → [l+k, l+2k-1]
* …
* Last chunk may be smaller

---

# Complexity

**Let:**
* `U` = number of messages for a user (≤ 100)

- **Time Complexity**
	* receiveMessage → `O(1)`
	* getMessageCount → `O(U)`

- **Space Complexity**
	* `O(total messages)` ≤ `O(100)`

This is optimal under constraints.

---

# Code

```java
import java.util.*;

public class Solution {

    private static Map<String, List<Integer>> userMessages = new HashMap<>();

    public static void receiveMessage(String user, int time) {
        userMessages.computeIfAbsent(user, k -> new ArrayList<>()).add(time);
    }

    public static ArrayList<Integer> getMessageCount(int l, int r, String user, int k) {

        int totalRange = r - l + 1;
        int chunks = (totalRange + k - 1) / k;

        int[] counts = new int[chunks];

        if (!userMessages.containsKey(user)) {
            ArrayList<Integer> result = new ArrayList<>();
            for (int i = 0; i < chunks; i++) result.add(0);
            return result;
        }

        for (int time : userMessages.get(user)) {
            if (time < l || time > r) continue;
            int index = (time - l) / k;
            counts[index]++;
        }

        ArrayList<Integer> result = new ArrayList<>();
        for (int c : counts) result.add(c);
        return result;
    }
};

```

---

# Example Walkthrough

**Input**

```
Messages for user "abc": [3, 5, 9]
Query: l=5, r=10, k=3
```

**Chunks:**

* Chunk 0 → [5, 6, 7]
* Chunk 1 → [8, 9, 10]

**Process timestamps:**

* 3 → ignored
* 5 → (5-5)/3 = 0 → chunk 0
* 9 → (9-5)/3 = 1 → chunk 1

**Result:**

```
[1, 1]
```

---

## Core insight

This problem is not about optimization.
It is about **correct bucketing with precise index math**.

---
