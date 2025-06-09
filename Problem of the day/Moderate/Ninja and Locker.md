# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
	public static int savedCash(int n, ArrayList<Integer> cash, int m, ArrayList<Integer> locker) {
		ArrayList<Integer> minHeights = new ArrayList<>(m);
		int min = locker.get(0);
		minHeights.add(min);

		for (int i = 1; i < m; i++) {
			min = Math.min(min, locker.get(i));
			minHeights.add(min);
		}

		int i = 0, j = m-1;

	Collections.sort(cash);

	int count = 0;

	while (i < n && j >= 0) {
		if (cash.get(i) <= minHeights.get(j)) {
			count++;
			i++;
			j--;
		} else {
			j--;
		}
	}
	return count;
	}
}

```