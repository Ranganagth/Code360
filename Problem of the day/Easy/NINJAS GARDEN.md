# Solution

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
	public static int ninjaGarden (ArrayList<Integer> flowers, int n, int k) {
		int[] days = new int[n + 1];

		for (int i = 0; i < n; i++) {
			days[flowers.get(i)] = i + 1;
		}
			int answer = Integer.MAX_VALUE;
			int left = 1, right = k + 2;

			while (right <= n) {
				boolean valid = true;

				for (int i = left + 1; i < right; i++) {
					if(days[i] < Math.max(days[left], days[right])) {
						valid = false;
						left = i;
						right = i + k + 1;
						break;
					}
				}

				if (valid) {
					answer = Math.min(answer, Math.max(days[left], days[right]));
					left = right;
					right = left + k + 1;
				}
			}

			return answer == Integer.MAX_VALUE ? -1 : answer;
		}
	}
	
```