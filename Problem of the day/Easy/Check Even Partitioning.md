# Solution

```java
import java.util.* ;
import java.io.*; 

public class Solution {
	public static boolean checkEvenPartitioning(int n) {
		
		return n > 2 && n % 2 == 0;
	}

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int T = sc.nextInt();

		while (T-- > 0) {
			int n = sc.nextInt();

			if (checkEvenPartitioning(n)) {
				System.out.println("YES");
			} else {
				System.out.println("NO");
			}
		}

		sc.close();
	}
}

```