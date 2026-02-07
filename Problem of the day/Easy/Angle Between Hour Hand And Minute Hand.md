### Intuition

Minute hand moves **360° in 60 minutes → 6° per minute**.
Hour hand moves **360° in 12 hours → 30° per hour**, and also moves continuously **0.5° per minute**.

So:

* Minute hand angle = `6 × minute`
* Hour hand angle = `30 × hour + 0.5 × minute`

Angle between them = absolute difference of the two.
Since there are two angles on a circle, the required answer is:

```
min(diff, 360 - diff)
```

Finally return the **floor value**.

---

### Approach

1. If hour = 12, treat as 0.
2. Compute:

   * hourAngle = `30 * hour + 0.5 * minute`
   * minuteAngle = `6 * minute`
3. diff = `|hourAngle - minuteAngle|`
4. result = `min(diff, 360 - diff)`
5. Return `floor(result)`.

---

### Complexity

* Time: `O(1)`
* Space: `O(1)`

---

### Java Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int findAngle(int hour, int minute) {

        hour = hour % 12;

        double hourAngle = 30 * hour + 0.5 * minute;
        double minuteAngle = 6 * minute;

        double diff = Math.abs(hourAngle - minuteAngle);
        double ans = Math.min(diff, 360 - diff);

        return (int)Math.floor(ans);
    }
}
```

---

### Example Walkthrough

**Input:** 8 55

* Hour angle = `30×8 + 0.5×55 = 240 + 27.5 = 267.5`
* Minute angle = `6×55 = 330`
* diff = `|330 − 267.5| = 62.5`
* Other angle = `360 − 62.5 = 297.5`

Minimum = `62.5`
Floor = **62**

---

# Solution in JavaScript

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

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

function calculateMinAngle(H, M) {
    let minute_angle = M * 6;
    let hour_angle = H % 12 * 30 + M * 0.5;
    let angle = Math.abs(hour_angle - minute_angle);
    return Math.floor(Math.min(angle, 360 - angle));
}


function main() {
    let T = parseInt(readLine());
    for (let t = 0; t < T; t++) {
        const [H, M] = readLine().trim().split(" ").map(Number);
        console.log(calculateMinAngle(H, M));
    }

}

```