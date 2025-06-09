# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function longestMountainSubarray(N, arr) {
    if (N < 3) {
        return 0;
    }

    let maxLength = 0;

    for (let i = 1; i < N - 1; i++) {
        if (arr[i] > arr[i - 1] && arr[i] > arr[i + 1]) {
            let left = i - 1;
            let right = i + 1;

            while (left > 0 && arr[left] > arr[left - 1]) {
                left--;
            }

            while (right < N - 1 && arr[right] > arr[right + 1]) {
                right++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }
    }

    return maxLength;
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

    let T = parseInt(readLine());
    for (let t = 0; t < T; t++) {
        let N = parseInt(readLine());
        const arr = readLine().replace(/\s+$/g, '').split(' ').map(arrTemp => parseInt(arrTemp));

        let result = longestMountainSubarray(N, arr);
        console.log(result);
    }

}

```