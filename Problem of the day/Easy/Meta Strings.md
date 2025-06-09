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

function areMetaStrings(str1, str2) {
    if (str1.length !== str2.length || str1 === str2) return "NO";

    let diffs = [];

    for (let i = 0; i < str1.length; i++) {
        if (str1[i] !== str2[i]) {
            diffs.push(i);
            if (diffs.length > 2) return "NO";
        }
    }

    if (diffs.length !== 2) return "NO";

    let [i, j] = diffs;

    return (str1[i] === str2[j] && str1[j] === str2[i]) ? "YES" : "NO";
}

function main() {
    let T = parseInt(readLine());
    for (let t = 0; t < T; t++) {
        let str1 = readLine();
        let str2 = readLine();
        console.log(areMetaStrings(str1, str2));
    }


    /* Read your input here 
    eg: For string variables:   let str = String(readLine()); 
        For int variables: let var_name = parseInt(readLine());
        For arrays : const arr = readLine().replace(/\s+$/g, '').split(' ');
    */

    /*
    Call your function with the input/parameters read above
    eg: let x = example(var_name, arr);
    */

    /*
    Log your output here 
    eg: console.log(x);
    */

}

```