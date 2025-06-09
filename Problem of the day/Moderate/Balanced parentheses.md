# Solution

```java
import java.util.* ;
import java.io.*; 

public class Solution {
	public static List<String> balancedParantheses(int n) {
		List<String> result = new ArrayList<>();
		generateParantheses(n, 0, 0, "", result);
		return result;
	}

	private static void generateParantheses(int n, int open, int close, String current, List<String> result) {
		if (current.length() == 2 * n) {
			result.add(current);
			return;
		}

		if (open < n) {
			generateParantheses(n, open + 1, close, current + "(", result);
		}

		if (close < open) {
			generateParantheses(n, open, close + 1, current + ")", result);
		}
	}
}

```

# Solution - 2

```java
import java.util.* ;
import java.io.*; 

public class Solution {
	public static List<String> balancedParantheses(int n) {
		List<String> result = new ArrayList<>();
		generateParentheses(n, 0, 0, "", result);
		return result;
	}

	private static void generateParentheses(int n, int open, int close, String current, List<String> result) {
		if (current.length() == 2 * n) {
			result.add(current);
			return;
		}

		if (open < n) {
			generateParentheses(n, open + 1, close, current + "(", result);
		}
		if (close < open) {
			generateParentheses(n, open, close + 1, current + ")", result);
		}
	}

	public static void main(String[] args) throws IOException {
		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();

		while (T-- > 0) {
			int n = sc.nextInt();
			List<String> res = balancedParantheses(n);
			for (String s : res) {
				System.out.print(s + " ");
			}
			System.out.println();
		}

		sc.close();
	}
}

```