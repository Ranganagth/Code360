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