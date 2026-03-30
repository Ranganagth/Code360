# Intuition

Pattern rules:

* Row `i` has `i` numbers
* Numbers increase sequentially and wrap after 9 → `(num % 9) + 1`
* Right alignment → leading spaces = `n - i`

Since function returns numbers (not spaces), only build numeric structure.

---

# Approach

1. Maintain a global counter starting from `1`
2. For each row `i`:

   * create list of size `i`
   * fill values using counter (wrap after 9)
3. Increment counter continuously across rows

---

# Complexity

* **Time complexity:**
$$O(N^2)
$$

* **Space complexity:**
$$O(N^2)
$$
---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

	public static ArrayList<ArrayList<Integer>> numberPattern(int n) {

		ArrayList<ArrayList<Integer>> result = new ArrayList<>();

		int num = 1;

		for (int i = 1; i <= n; i++) {

			ArrayList<Integer> row = new ArrayList<>();

			// add placeholders for spaces
			for (int s = 0; s < n - i; s++) {
				row.add(-1);
			}

			// add numbers
			for (int j = 0; j < i; j++) {
				row.add(num);
				num++;

				if (num == 10) num = 1;
			}

			result.add(row);
		}

		return result;
	}
};

```

---

# Example walkthrough

For `N = 4`:

```text
Row 1 → [1]
Row 2 → [2,3]
Row 3 → [4,5,6]
Row 4 → [7,8,9,1]
```
