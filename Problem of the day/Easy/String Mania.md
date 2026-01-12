# Intuition

This is essentially a **lexicographical comparison** problem, similar to how strings are compared in dictionaries or how `compareTo()` works in Java.  
We need to compare both strings character by character.  
- As soon as a mismatch is found, the character that is lexicographically larger determines the better string.  
- If all characters match up to the shorter string’s length, then the longer string is considered better.

---

# Approach

1. Iterate through both strings up to the minimum of their lengths.  
2. At each index:
   - If characters differ, return `1` if `str1[i] > str2[i]`, else return `-1`.
3. If the loop completes without differences:
   - If `n > m`, `str1` is better → return `1`
   - If `n < m`, `str2` is better → return `-1`
   - If `n == m`, both are same → return `0`

---

# Complexity

- **Time complexity**:  
  $O(\min(n, m))$ - We compare character-by-character until mismatch or end.

- **Space complexity**:  
  `O(1)` - Constant extra space.

---

# Code 

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int stringMania(int n, int m, String str1, String str2) {
        int minLen = Math.min(n, m);

        for (int i = 0; i < minLen; i++) {
            if (str1.charAt(i) != str2.charAt(i)) {
                return str1.charAt(i) > str2.charAt(i) ? 1 : -1;
            }
        }

        if (n == m) return 0;
        return n > m ? 1 : -1;
    }
};

```


### Sample Driver Code (for multiple test cases)

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();

        while (T-- > 0) {
            int n = sc.nextInt();
            int m = sc.nextInt();
            String str1 = sc.next();
            String str2 = sc.next();

            int result = Solution.stringMania(n, m, str1, str2);
            System.out.println(result);
        }
    }
}
```

---

# Example and Explanation Walkthrough

**Input**:
```
3
2 3
ez
ehz
5 5
acefi
acefi
3 5
ags
agtaa
```

**Output**:
```
1
0
-1
```

**Explanation**:

1. `ez` vs `ehz`: Both start with `e`, then `z > h`, so `ez` is better → output `1`
2. Same strings → output `0`
3. `ags` vs `agtaa`: Match till 2nd index. Then `s < t`? No, but `ags` ends, and `agtaa` continues → `str2` is better → output `-1`
