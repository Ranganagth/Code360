# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function findMedianSortedArrays(a, b) {
    if (a.length > b.length) return findMedianSortedArrays(b, a);

    let n = a.length, m = b.length;
    let low = 0, high = n;

    while (low <= high) {
        let cut1 = Math.floor((low + high) / 2);
        let cut2 = Math.floor((n + m + 1) / 2) - cut1;

        let left1 = cut1 === 0 ? -Infinity : a[cut1 - 1];
        let left2 = cut2 === 0 ? -Infinity : b[cut2 - 1];

        let right1 = cut1 === n ? Infinity : a[cut1];
        let right2 = cut2 === m ? Infinity : b[cut2];

        if (left1 <= right2 && left2 <= right1) {
            if ((n + m) % 2 === 0) {
                return (Math.max(left1, left2) + Math.min(right1, right2)) / 2;
            } else {
                return Math.max(left1, left2);
            }
        } else if (left1 > right2) {
            high = cut1 - 1;
        } else {
            low = cut1 + 1;
        }
    }
    return 0;
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

    const [n, m] = readLine().trim().split(/\s+/).map(Number);
    const a = readLine().trim().split(/\s+/).map(Number);
    const b = readLine().trim().split(/\s+/).map(Number);

    const result = findMedianSortedArrays(a, b);
    console.log(result.toFixed(1));
}

```