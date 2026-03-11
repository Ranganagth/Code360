# Intuition

Only the elements of `arr1` that **already appear in `arr2` in correct order** help form the subsequence.
Any remaining elements of `arr1` must be **inserted**.

Thus:

```id="s7xg2c"
minimum insertions = n - length of longest subsequence of arr1 that appears in arr2
```

Since `arr1` has **distinct elements**, map each value of `arr1` to its index.
Then scan `arr2` and convert elements that exist in `arr1` to their indices.
The problem becomes finding the **Longest Increasing Subsequence (LIS)** of these indices.

---

# Approach

1. Create a hashmap:

   ```
   value → index in arr1
   ```
2. Traverse `arr2`:

   * If element exists in `arr1`, add its mapped index to a list.
3. Find LIS length of this index list.
4. Result:

```
insertions = n - LIS
```

Use binary search LIS for (O(M \log N)).

---

# Complexity

* **Time complexity:** (O(M \log N))
* **Space complexity:** (O(N))

---

# Code

```java
import java.util.*;
import java.io.*;
import java.util.ArrayList;

public class Solution 
{
	public static int minimumInsertions(int n, int m, ArrayList<Integer> arr1, ArrayList<Integer> arr2) 
	{
		HashMap<Integer,Integer> map = new HashMap<>();

		for (int i = 0; i < n; i++)
			map.put(arr1.get(i), i);

		ArrayList<Integer> seq = new ArrayList<>();

		for (int val : arr2)
			if (map.containsKey(val))
				seq.add(map.get(val));

		ArrayList<Integer> lis = new ArrayList<>();

		for (int x : seq)
		{
			int pos = Collections.binarySearch(lis, x);

			if (pos < 0) pos = -(pos + 1);

			if (pos == lis.size())
				lis.add(x);
			else
				lis.set(pos, x);
		}

		return n - lis.size();
	}
};

```

---

# Example walkthrough with explanation

Example:

```id="23m5n6"
arr1 = [1,2,3]
arr2 = [3,2,1,4,7]
```

Mapping:

```id="7ldu6n"
1 → 0
2 → 1
3 → 2
```

Convert `arr2` to indices:

```id="a1og0y"
[2,1,0]
```

LIS:

```id="y3n9pr"
length = 1
```

Insertions needed:

```id="dsb9se"
3 - 1 = 2
```
