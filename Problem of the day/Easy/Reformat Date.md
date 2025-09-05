# Intuition

The problem is purely about **string parsing and formatting**:
- The input date has a **human-friendly format** (`"27th Apr 1998"`).  
- We need to convert it into a **machine-friendly format** (`"1998-04-27"`).  

So the key is to:  
1. Extract `day`, `month`, and `year` from the string.  
2. Convert them into proper **numeric, fixed-width** strings.  
3. Concatenate in the required order.

---

# Approach

1. **Split Input**  
   Split the given string into three parts using `" "` (space):  
   - `dayStr` → `"27th"`  
   - `monthStr` → `"Apr"`  
   - `yearStr` → `"1998"`

2. **Parse Day**  
   - Days are always written as `number + suffix` (`1st`, `2nd`, `3rd`, `4th` …).  
   - Remove the last **two characters** (`st`, `nd`, `rd`, `th`) to isolate the number.  
   - Convert to integer.  
   - Reformat into **2-digit string** (e.g., `"1"` → `"01"`, `"27"` → `"27"`).

3. **Parse Month**  
   - Create a lookup map:  
     ```
     Jan → 01, Feb → 02, ..., Dec → 12
     ```
   - Use this map to convert the month abbreviation into **2-digit numeric string**.

4. **Year**  
   - Already in `YYYY` format, so use directly.

5. **Combine**  
   Format as `"YYYY-MM-DD"`.

---

# Complexity Analysis

- **Time Complexity:**  
  Each input string has a constant size (day, month, year).  
  String splitting, substring, and lookups are **O(1)**.  
  For `T` test cases, complexity is **O(T)**.

- **Space Complexity:**  
  The month map is fixed size (`12` entries → O(1)).  
  No extra data structures dependent on input size.  
  So, total space is **O(1)**.

---
# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static String reformDate(String s) {
        // Step 1: Split into parts
        String[] parts = s.split(" ");
        String dayStr = parts[0];
        String monthStr = parts[1];
        String yearStr = parts[2];
        
        // Step 2: Process day (remove suffix, format as 2-digit)
        String dayNumStr = dayStr.substring(0, dayStr.length() - 2); 
        int day = Integer.parseInt(dayNumStr);
        String dayFormatted = String.format("%02d", day);

        // Step 3: Map month
        Map<String, String> monthMap = new HashMap<>();
        monthMap.put("Jan", "01");
        monthMap.put("Feb", "02");
        monthMap.put("Mar", "03");
        monthMap.put("Apr", "04");
        monthMap.put("May", "05");
        monthMap.put("Jun", "06");
        monthMap.put("Jul", "07");
        monthMap.put("Aug", "08");
        monthMap.put("Sep", "09");
        monthMap.put("Oct", "10");
        monthMap.put("Nov", "11");
        monthMap.put("Dec", "12");

        String monthFormatted = monthMap.get(monthStr);

        // Step 4: Year
        String yearFormatted = yearStr;

        // Step 5: Combine
        return yearFormatted + "-" + monthFormatted + "-" + dayFormatted;
    }
}
```

---

## Example Walkthrough

Input:
```
27th Apr 1998
```

**Step 1: Split**  
- `dayStr = "27th"`  
- `monthStr = "Apr"`  
- `yearStr = "1998"`

**Step 2: Parse Day**  
- Remove suffix: `"27th".substring(0, 2) = "27"`  
- Convert to integer: `27`  
- Format 2-digit: `"27"`

**Step 3: Parse Month**  
- Lookup `"Apr"` → `"04"`

**Step 4: Year**  
- Already `"1998"`

**Step 5: Combine**  
- `"1998-04-27"`

Output:
```
1998-04-27
```

---
