# Solution

```java
import java.io.*;
import java.util.* ;
import java.util.*;

public class Solution {
	public static ArrayList<Integer> getAllDivisors(int n){
		ArrayList<Integer> divisors = new ArrayList<>();

		for (int i = 1; i * i <= n; i++) {
			if (n % i == 0) {
				divisors.add(i);
				if (i != n /i) {
					divisors.add(n / i);
				}
			}
		}

		Collections.sort(divisors);
		return divisors;
	}

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int T = scanner.nextInt();

		while (T-- > 0) {
			int n = scanner.nextInt();
			ArrayList<Integer> result = getAllDivisors(n);

			for(int num: result) {
				System.out.print(num + " ");
			}
			System.out.println();
		}
		scanner.close();
	}
}

```