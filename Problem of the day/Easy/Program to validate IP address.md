# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function isValidIPv4(ip) {
    const parts = ip.split('.');
    if (parts.length !== 4) return false;

    for (const part of parts) {
        if (!/^\d+$/.test(part)) return false;

        const num = parseInt(part);
        if (num < 0 || num > 255) return false;

        if (part.length > 1 && part[0] === '0') return false;
    }
    return true;
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
    for (let i = 0; i < T; i++) {
        const ip = readLine().trim();
        const result = isValidIPv4(ip);
        console.log(result ? 'True' : 'False');
    }

}

```