# Solution

```java
import java.util.* ;
import java.io.*; 

public class Solution {

	public static int numberOfFlips(int a, int b) {
		int xor = a ^ b;
		int count = 0;

		while (xor != 0) {
			count += xor & 1;
			xor >>= 1;
		}

		return count;
	}

}

```