# Solution

```java
public class Solution{
    public static int missingNumber(int n, int []arr){
        int result = 0;

        for (int num : arr) {
            result ^= num;
        }

        return result;
    }
}

```