# Solution

```java
import java.util.*;
import java.io.*;
import java.math.BigInteger;

public class Solution {
	public static void factorial(int n) {
		BigInteger result = BigInteger.ONE;

		for(int i = 1; i <= n; i++) {
			result = result.multiply(BigInteger.valueOf(i));
		}

		System.out.println(result);
	}
}

```