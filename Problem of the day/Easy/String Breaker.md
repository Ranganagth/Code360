# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function minBreaksToFormWords(s, dict) {
    const n = s.length;
    const dp = new Array(n + 1).fill(Infinity);
    dp[0] = -1;

    for (let i = 1; i <= n; i++) {
        for (let j = 0; j < i; j++) {
            const word = s.substring(j, i);
            if (dict.has(word) && dp[j] !== Infinity) {
                dp[i] = Math.min(dp[i], dp[j] + 1);
            }
        }
    }

    return dp[n] === Infinity ? -1 : dp[n];
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
        const S = readLine().trim();
        const N = parseInt(readLine());
        const dict = new Set();

        for (let i = 0; i < N; i++) {
            dict.add(readLine().trim());
        }

        const result = dict.has(S) ? 0 : minBreaksToFormWords(S, dict);

        console.log(result);
    }

}

```