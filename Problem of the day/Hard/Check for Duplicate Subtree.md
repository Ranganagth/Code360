# Solution

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function Node(val) {
    this.val = val;
    this.left = null;
    this.right = null;
}

function buildTree(arr) {
    if (!arr.length || arr[0] === "#") return null;

    const root = new Node(arr[0]);
    const queue = [root];
    let i = 1;

    while (queue.length && i < arr.length) {
        const current = queue.shift();

        if (arr[i] !== "#") {
            current.left = new Node(arr[i]);
            queue.push(current.left);
        }
        i++;

        if (i < arr.length && arr[i] !== "#") {
            current.right = new Node(arr[i]);
            queue.push(current.right);
        }
        i++;
    }

    return root;
}

function hasDuplicateSubtree(root) {
    const map = new Map();
    let found = false;

    function serialize(node) {
        if (!node) return "#";

        const left = serialize(node.left);
        const right = serialize(node.right);

        const serial = `${node.val},${left},${right}`;

        if (serial !== `${node.val},#,#`) {
            map.set(serial, (map.get(serial) || 0) + 1);
            if (map.get(serial) === 2) found = true;
        }

        return serial;
    }

    serialize(root);
    return found;
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

    while (T--) {
        let N = parseInt(readLine());
        let values = [];

        while (values.length < N || values[values.length - 1] !== "#" || values.length < 2 * N - 1) {
            const line = readLine();
            if (!line) break;
            values = values.concat(line.trim().split(/\s+/));
        }

        const root = buildTree(values);
        console.log(hasDuplicateSubtree(root) ? "True" : "False");
    }
}

```