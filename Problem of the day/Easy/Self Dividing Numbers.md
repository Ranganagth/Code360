# Solution

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
    public static ArrayList<Integer> findAllSelfDividingNumbers(int lower, int upper) {
        ArrayList<Integer> result = new ArrayList<>();

        for (int i = lower; i <= upper; i++) {
            if (isSelfDividing(i)) {
                result.add(i);
            }
        }

        return result;
    }

    private static boolean isSelfDividing(int num) {
        int original = num;

        while (num > 0) {
            int digit = num % 10;
            if (digit == 0 || original % digit != 0) {
                return false;
            }

            num /= 10;
        }

        return true;
    }
}

```