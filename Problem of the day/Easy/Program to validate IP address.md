# Intuition

An IPv4 address is valid if:

1. It consists of exactly **four parts** separated by `.`.
2. Each part must be a **number** without extra characters.
3. Each part must be in the range **0–255**.
4. Leading zeros like `"01"` are invalid unless the number is exactly `"0"`.

---

# Approach

1. **Split the string** by `"."`.
2. **Check length**: Must have exactly 4 parts.
3. For each part:

   * Ensure it's not empty.
   * Ensure it contains only digits.
   * Ensure it does not have leading zeros (unless it's `"0"`).
   * Parse to integer and check if `0 <= num <= 255`.
4. If all checks pass → return `true`, else `false`.

---

# Complexity

* Splitting + validation of 4 parts: **O(L)** per IP, where `L` = length of string (≤ 50).
* For up to **T = 10^4** inputs → worst case ~500,000 characters processed, well within 1 second.

---

# Code

## Solution 1 in Java

```java
import java.util.* ;
import java.io.*; 

public class Solution {

    public static boolean isValidIPv4(String ipAddress) {
        // Split by "."
        String[] parts = ipAddress.split("\\.");

        // Must have exactly 4 parts
        if (parts.length != 4) return false;

        for (String part : parts) {
            // Empty part (like "1..1.1")
            if (part.length() == 0) return false;

            // Check if numeric
            for (char c : part.toCharArray()) {
                if (!Character.isDigit(c)) return false;
            }

            // Leading zero check
            if (part.length() > 1 && part.charAt(0) == '0') return false;

            try {
                int value = Integer.parseInt(part);
                if (value < 0 || value > 255) return false;
            } catch (NumberFormatException e) {
                return false;
            }
        }

        return true;
    }
};

```

---

## Solution 2 in JavaScript

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function isValidIPv4(ip) {
    const parts = ip.split('.');
    if (parts.length !== 4) return false;

    for (const part of parts) {
        if (!/^\d+$/.test(part)) return false;

        const num = parseInt(part);
        if (num < 0 || num > 255) return false;

        if (part.length > 1 && part[0] === '0') return false;
    }
    return true;
}


process.stdin.resume();
process.stdin.setEncoding('ascii');

var input_stdin = "";
var input_stdin_array = "";
var input_currentline = 0;

process.stdin.on('data', function (data) {
input_stdin += data;
});

process.stdin.on('end', function () {
input_stdin_array = input_stdin.split("\n");
main();    
});

function readLine() {
return input_stdin_array[input_currentline++];
}


function main() {

    const T = parseInt(readLine());
    for (let i = 0; i < T; i++) {
        const ip = readLine().trim();
        const result = isValidIPv4(ip);
        console.log(result ? 'True' : 'False');
    }

}

```

---

## Example Walkthrough

Input: `"1.1.1.250"`

* Split → `["1","1","1","250"]` → 4 parts 
* All are digits 
* `"250"` is within 0–255 
* Valid → Output: **True**

Input: `"122.0.330.0"`

* Split → `["122","0","330","0"]` 
* `"330"` > 255
* Invalid → Output: **False**

---
