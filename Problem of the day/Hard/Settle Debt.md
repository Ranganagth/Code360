# Intuition

Each transaction changes the **net balance** of people:

* giver loses money → negative balance
* receiver gains money → positive balance

After processing all transactions, only the **net balances** matter.

Goal: settle all balances using the **minimum number of transfers**.

Since `N ≤ 9`, the number of people involved is small. A **DFS/backtracking** approach can try settling one person's debt with others and recursively minimize the remaining transactions.

Key idea:

* pick the first non-zero balance
* try settling it with every opposite-sign balance
* recurse and track the minimum operations.

---

# Approach

1. Compute net balance for each person.
2. Remove zero balances.
3. Use DFS:

   * skip settled indices
   * try settling current debt with any opposite-sign balance
   * recursively solve the remaining problem
4. Track the minimum number of transactions.

---

# Complexity

* **Time complexity:** (O(n!)) worst case due to backtracking
* **Space complexity:** (O(n))

---

# Code

```java
import java.util.*;
import java.io.*;
import java.util.ArrayList;

public class Solution {

	public static int settleDebt(ArrayList<ArrayList<Integer>> arr) {

		HashMap<Integer, Integer> map = new HashMap<>();

		for (ArrayList<Integer> t : arr) {
			int x = t.get(0);
			int y = t.get(1);
			int z = t.get(2);

			map.put(x, map.getOrDefault(x, 0) - z);
			map.put(y, map.getOrDefault(y, 0) + z);
		}

		ArrayList<Integer> debt = new ArrayList<>();

		for (int val : map.values())
			if (val != 0)
				debt.add(val);

		return dfs(0, debt);
	}

	static int dfs(int start, ArrayList<Integer> debt) {

		while (start < debt.size() && debt.get(start) == 0)
			start++;

		if (start == debt.size())
			return 0;

		int ans = Integer.MAX_VALUE;

		for (int i = start + 1; i < debt.size(); i++) {

			if (debt.get(start) * debt.get(i) < 0) {

				debt.set(i, debt.get(i) + debt.get(start));

				ans = Math.min(ans, 1 + dfs(start + 1, debt));

				debt.set(i, debt.get(i) - debt.get(start));

				if (debt.get(i) + debt.get(start) == 0)
					break;
			}
		}

		return ans;
	}
};

```

---

# Example walkthrough

Transactions:

```
[0,1,10]
[2,0,5]
```

Balances:

```
0 → -5
1 → +10
2 → -5
```

Settle:

```
1 pays 0 → 5
1 pays 2 → 5
```

Total transactions:

```
2
```
