# Intuition

* To reverse words in a string in-place:
  1. First, reverse the entire string.
  2. Then reverse each word individually to correct their characters.

Example:
Input: `"when all else fails reboot"`
Step 1 (reverse all): `"toober sliaf esle lla nehw"`
Step 2 (reverse each word): `"reboot fails else all when"` 

---

# Approach

1. Convert the string to a character array (since Java strings are immutable).
2. Reverse the entire array.
3. Traverse the array and reverse each word individually by detecting spaces.
4. Convert the modified char array back to string and return.

---

# Complexity Analysis

* **Time:** O(N) — every character is visited a constant number of times.
* **Space:** O(N) — due to character array (in-place conceptually; technically needed due to Java string immutability).

---

# Code

```java
public class Solution {
    
    public static String reverseOrderWords(String str) {
        char[] arr = str.toCharArray();

        // Step 1: Reverse entire string
        reverse(arr, 0, arr.length - 1);

        // Step 2: Reverse each word in the reversed string
        int start = 0;
        for (int i = 0; i <= arr.length; i++) {
            if (i == arr.length || arr[i] == ' ') {
                reverse(arr, start, i - 1);
                start = i + 1;
            }
        }

        return new String(arr);
    }

    private static void reverse(char[] arr, int left, int right) {
        while (left < right) {
            char temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
}

```

---

### **Example and Walkthrough**

#### Input:

```java
String str = "when all else fails reboot";
System.out.println(reverseOrderWords(str));
```

#### Output:

```
reboot fails else all when
```

#### Explanation:

1. Original: `"when all else fails reboot"`
2. After full reverse: `"toober sliaf esle lla nehw"`
3. After word-wise reverse: `"reboot fails else all when"`

---
