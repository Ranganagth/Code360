# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
	public static int minSubArrayLen(int arr[], int target, int n) {
		int minLen = Integer.MAX_VALUE;
		int start = 0, sum = 0;

		for (int end = 0; end < n; end++) {
			sum += arr[end];

			while (sum > target) {
				minLen = Math.min(minLen, end - start + 1);
				sum -= arr[start++];
			}
		}

		return (minLen == Integer.MAX_VALUE) ? 0 : minLen;
	}

	public static void main(String[] args) throws IOException {
		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();

		while(T-- > 0) {
			int n = sc.nextInt();
			int x = sc.nextInt();
			int[] arr = new int[n];

			for (int i = 0; i < n; i++) {
				arr[i] = sc.nextInt();
			}

			System.out.println(minSubArrayLen(arr, x, n));
		}

		sc.close();
	}
}

```