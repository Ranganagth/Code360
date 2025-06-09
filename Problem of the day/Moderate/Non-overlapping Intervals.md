# Solution

```javascript
function minimumReschedules(n, intervals) {
    intervals.sort((a, b) => a[1] - b[1]);

    let count = 0;
    let end = -Infinity;

    for (let i = 0; i < n; i++) {
        const [start, finish] = intervals[i];
        if (start >= end) {
            end = finish;
            count++
        }
    }
    return n - count;
}

module.exports.minimumReschedules = minimumReschedules;

```