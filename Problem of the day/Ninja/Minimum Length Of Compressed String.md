# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function getLength(count) {
    if (count === 1) return 1;
    if (count < 10) return 2;
    if (count < 100) return 3;
    return 4;
}

function minEncodedLength(s, k) {
    const n = s.length;

    const dp = new Array(n + 1).fill(0).map(() => new Array(k + 1).fill(0).map(() => new Array(101).fill(Infinity)
    )
    );

    for (let del = 0; del <= k; del++) {
        for (let count = 0; count <= 100; count++) {
            dp[n][del][count] = 0;
        }
    }

    for (let i = n - 1; i >= 0; i--) {
        for (let del = 0; del <= k; del++) {
            for (let count = 0; count <= 100; count++) {
                if (del < k) {
                    dp[i][del][count] = dp[i + 1][del + 1][count];
                }

                let len = 0;
                for (let j = i, same = 0, diff = 0; j < n; j++) {
                    if (s[j] === s[i]) same++;
                    else diff++;

                    if (del + diff > k) break;

                    len = getLength(same);
                    dp[i][del][count] = Math.min(dp[i][del][count], len + dp[j + 1][del + diff][0]);
                }
            }
        }
    }

    return dp[0][0][0];
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
        let str = readLine().trim();
        let k = parseInt(readLine());
        console.log(minEncodedLength(str, k));
    }

}

```