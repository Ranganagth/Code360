# Solution (10/11 Test cases)

```java
import java.util.* ;
import java.io.*; 

public class Solution {

	public static boolean duplicateParanthesis(String expr) {
		Stack<Character> stack = new Stack<>();

		for (char ch : expr.toCharArray()) {
			if (ch == ')') {
				int elementsInside = 0;

				while (!stack.isEmpty && stack.peek() != '(') {
					stack.pop();
					elementsInside++;
				}

				if (stack.isEmpty()) {
					continue;
				}

				int numPopped = 0;
				while (!stack.isEmpty() && stack.peek() != '(') {
					char
				}
			}
		}
	}

}

```