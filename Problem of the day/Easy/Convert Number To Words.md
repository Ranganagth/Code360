# Intuition

Convert the number into words by breaking it into Indian-system components:

* Crore (10⁷)
* Lakh (10⁵)
* Thousand (10³)
* Last three digits (hundreds + tens + units)

Each segment is individually converted to words (1–999).
Concatenate non-zero segments, inserting **“and”** only when lower two-digit part exists after higher segments (e.g., `"two thousand and five"`).

---

# Approach

1. Predefine lookup arrays for:

   * numbers below 20
   * tens
2. Convert:

   * 1–99 → `twoDigit()`
   * 1–999 → `threeDigit()` including "hundred and" rule
3. Break input number into:

   * crore
   * lakh
   * thousand
   * last three digits
4. Append each non-zero part with its scale word.
5. Insert **"and"** only when needed:

   * Higher blocks exist (crore/lakh/thousand)
   * And last block < 100

---

# Complexity

* **Time:** O(1) per query
  (Fixed operations, fixed decomposition)
* **Space:** O(1)

---

# Code

```java
import java.util.*;

public class Solution {

    static final String[] below20 = {
        "", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine",
        "ten", "eleven", "twelve", "thirteen", "fourteen", "fifteen", "sixteen",
        "seventeen", "eighteen", "nineteen"
    };

    static final String[] tens = {
        "", "", "twenty", "thirty", "forty", "fifty", "sixty", "seventy", "eighty", "ninety"
    };

    private static String twoDigit(long n) {
        if (n < 20) return below20[(int) n];
        long t = n / 10, u = n % 10;
        if (u == 0) return tens[(int) t];
        return tens[(int) t] + " " + below20[(int) u];
    }

    private static String threeDigit(long n) {
        long h = n / 100, rest = n % 100;

        if (h == 0) return twoDigit(rest);
        if (rest == 0) return below20[(int) h] + " hundred";

        return below20[(int) h] + " hundred and " + twoDigit(rest);
    }

    public static String handleAll(long n) {
        if (n == 0) return "zero";

        StringBuilder sb = new StringBuilder();

        long crore = n / 10000000;
        long lakh = (n / 100000) % 100;
        long thousand = (n / 1000) % 100;
        long last = n % 1000;

        if (crore > 0)
            sb.append(threeDigit(crore)).append(" crore ");

        if (lakh > 0)
            sb.append(threeDigit(lakh)).append(" lakh ");

        if (thousand > 0)
            sb.append(threeDigit(thousand)).append(" thousand ");

        boolean hasHigher = (crore > 0 || lakh > 0 || thousand > 0);

        if (last > 0) {
            if (hasHigher && last < 100)
                sb.append("and ");

            sb.append(threeDigit(last));
        }

        return sb.toString().trim();
    }
};

```

---

# Example walkthrough with explanation

### Input

`555093`

Break into segments:

* crore = 0
* lakh = 5
* thousand = 55
* last = 93

Conversions:

* `five lakh`
* `fifty five thousand`
* Insert **“and”** before last because last < 100
* `ninety three`

Final:
**"five lakh fifty five thousand and ninety three"**

Matches expected output.

---
