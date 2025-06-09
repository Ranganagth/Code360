# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function minDistance(arr, x, y) {
    let n = arr.length;
    let minDistance = Infinity;
    let lastIndexX = -1;
    let lastIndexY = -1;

    let foundX = false;
    let foundY = false;

    for (let i = 0; i < n; i++) {
        if (arr[i] === x) {
            lastIndexX = i;
            foundX = true;

            if (lastIndexY !== -1) {
                minDistance = Math.min(minDistance, Math.abs(lastIndexX - lastIndexY));
            }
        } else if (arr[i] === y) {
            lastIndexY = i;
            foundY = true;

            if (lastIndexX !== -1) {
                minDistance = Math.min(minDistance, Math.abs(lastIndexY - lastIndexX));
            }
        }
    }

    if (!foundX || !foundY) {
        return -1;
    }

    return minDistance;
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
    for (let t = 0; t < T; t++) {
        const [N, X, Y] = readLine().split(' ').map(Number);
        const ARR = readLine().split(' ').map(Number);

        console.log(minDistance(ARR, X, Y));
    }

}

```