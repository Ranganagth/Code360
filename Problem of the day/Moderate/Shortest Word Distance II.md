# Intuition

We want to find the **minimum distance** between any two occurrences of `book1` and `book2` in an array.

Instead of storing all indices and comparing, we can do it **in one pass** using two pointers: one for the last seen index of `book1` and one for `book2`.

---

# Approach

1. Traverse the array once.
2. When you find `book1`, store its index.
3. When you find `book2`, store its index.
4. If both indices are seen, calculate the distance and update the minimum.
5. Return the minimum distance.

---
# Complexity

* **Time Complexity:** `O(N)` per test case
* **Space Complexity:** `O(1)` — only two variables used

---

# Code

```java
import java.util.* ;
import java.io.*; 
public class Solution {

    public static int minimumDistance(String[] arr, String book1, String book2) {
   		int minimumDistance = Integer.MAX_VALUE;
        int index1 = -1, index2 = -1;

        for (int i = 0; i < arr.length; i++) {
            if (arr[i].equals(book1)) {
                index1 = i;
                if (index2 != -1) {
                    minimumDistance = Math.min(minimumDistance, Math.abs(index1 - index2));
                }
            } else if (arr[i].equals(book2)) {
                index2 = i;
                if (index1 != -1) {
                    minimumDistance = Math.min(minimumDistance, Math.abs(index1 - index2));
                }
            }
        }

        return minimumDistance;
    }   
}

```

---

### **Example Walkthrough**

#### Input:

```java
arr = ["eat", "code", "sleep", "repeat", "eat", "code", "sleep", "repeat"]
book1 = "eat", book2 = "repeat"
```

#### Tracking:

* `i=0` → "eat" → `index1 = 0`
* `i=3` → "repeat" → `index2 = 3` → `|3 - 0| = 3`
* `i=4` → "eat" → `index1 = 4` → `|4 - 3| = 1` → min updated
* `i=7` → "repeat" → `index2 = 7` → `|7 - 4| = 3`

Final minimum distance = **1**

---
