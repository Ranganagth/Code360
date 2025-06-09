# Solution

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

function findSumOfMaxMin(arr, n) {
    if (n === 1) return arr[0] * 2;

    let maxElement = -Infinity;
    let minElement = Infinity;

    for (let i = 0; i < n; i++) {
        if (arr[i] > maxElement) {
            maxElement = arr[i];
        }
        if (arr[i] < minElement) {
            minElement = arr[i];
        }
    }

    return maxElement + minElement;
}


function main() {

    const T = parseInt(readLine());

    for (let t = 0; t < T; t++) {
        const N = parseInt(readLine());
        const arr = readLine().split(' ').map(Number);
        const result = findSumOfMaxMin(arr, N);
        console.log(result);
    }

}

```