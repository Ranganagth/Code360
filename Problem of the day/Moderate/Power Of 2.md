# Solution 1

```java
import java.util.* ;
import java.io.*; 
public class Solution {

	private static String getDigitFrequency(int n) {
		char[] digits = Integer.toString(n).toCharArray();
		Arrays.sort(digits);
		return new String(digits);
	}
	public static boolean reorderedPowerOf2(int n) {
		HashSet<String> powerOfTwoFrequencies = new HashSet<>();

		for ( int i = 0; i < 31; i ++) {
			int powerofTwo = 1 << i;
			powerOfTwoFrequencies.add(getDigitFrequency(powerofTwo));
		}

		String nFrequency = getDigitFrequency(n);

		return powerOfTwoFrequencies.contains(nFrequency);
		}

		public static void main(String[] args) {
			Scanner scanner = new Scanner(System.in);
			int T = scanner.nextInt();

			while (T-- > 0) {
				int n = scanner.nextInt();
				System.out.println(reorderedPowerOf2(n) ? 1 : 0);
			}
			scanner.close();
		}
}

```


---

# Intuition

The goal is to determine whether the digits of a given number `N` can be **reordered** to form a **power of 2**.  
Instead of generating all permutations (which is expensive), we observe that **any valid permutation of digits** forming a power of 2 will have the **same frequency of digits** as that power of 2.  
So, we just need to compare the **digit frequency (sorted string)** of `N` with those of all powers of 2 within the valid range (up to `2^30` since `2^31` > 10⁹).

---

# Approach

1. Convert the number `N` to a character array, sort it, and store the result as a **canonical digit signature**.
2. Precompute the **sorted digit signatures** for all powers of 2 from `2^0` to `2^30`.
3. If any of those precomputed signatures matches the one for `N`, return `true` (print `1`); else return `false` (print `0`).

This avoids permutation generation and speeds up the checking process.

---

# Complexity

- **Time complexity**:  
    $O(T \cdot \log K)$
    where `T` is the number of test cases and `K` is the number of digits in the number (max 10). Since we check at most 31 powers of 2, this is effectively constant.
    
- **Space complexity**:  
    $O(1)$
    Aside from a few strings and arrays, we don't use additional space proportional to input size.
    

---

# Code

```java
import java.util.*;

public class Solution {
    public static boolean reorderedPowerOf2(int n) {
        String target = getSortedDigits(n);

        // Compare with all powers of 2 from 2^0 to 2^30
        for (int i = 0; i <= 30; i++) {
            int power = 1 << i;
            if (target.equals(getSortedDigits(power))) {
                return true;
            }
        }
        return false;
    }

    // Helper function to get sorted digits as canonical string
    private static String getSortedDigits(int num) {
        char[] digits = String.valueOf(num).toCharArray();
        Arrays.sort(digits);
        return new String(digits);
    }
}
```
