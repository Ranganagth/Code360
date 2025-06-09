# Solution

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
	public static ArrayList<ArrayList<Integer>> findPairs (int n, int m, ArrayList<Integer> arr1, ArrayList<Integer> arr2, int k) {
		ArrayList<ArrayList<Integer>> result = new ArrayList<>();

		PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> {
			int sumCompare = Integer.compare(a[0], b[0]);
			if (sumCompare == 0) return Integer.compare(arr1.get(a[1]), arr1.get(b[1]));
			return sumCompare;
		});

		for (int i = 0; i < Math.min(n, k); i++) {
			minHeap.offer(new int[]{arr1.get(i) + arr2.get(0), i, 0});
		}

		while (k-- > 0 && !minHeap.isEmpty()) {
			int[] current = minHeap.poll();
			int i = current[1], j = current[2];

			ArrayList<Integer> pair = new ArrayList<>();
			pair.add(arr1.get(i));
			pair.add(arr2.get(j));
			result.add(pair);

			if (j + 1 < m) {
				minHeap.offer(new int[]{arr1.get(i) + arr2.get(j + 1), i, j + 1});
			}
		}

		return result;
	}
}

```