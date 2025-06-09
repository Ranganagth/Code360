# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function minimminimumDifference(N) {
    let total = (N * (N + 1)) / 2;
    return total % 2;
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
        const N = parseInt(readLine());
        console.log(minimminimumDifference(N));
    }
}

```