## mutate

mutate 基本上就是 change 的意思，之所以叫 mutate，是因为它有“危险”的意思，警示你要格外小心。

## const

const 防止变量重新分配，但对象的 mutation 是制止不了的。网上对于 const 的实用性一直都是一个备受争议的话题，有的人说禁止 let，直接始终用 const;也有人说应该信任程序员重新分配自己的变量。

## Prototypes

- 当找到了对象自身的属性，将停止搜索原型链。
- hasOwnProperty 只会查找对象自身属性，不会搜索原型链。
- 原型链污染

```js
let obj = {};
obj.__proto__.smell = "banana";

let puzzle1 = {
  __proto__: obj,
};
let puzzle2 = {
  __proto__: obj,
};
console.log(puzzle1.smell); // "banana"
console.log(puzzle2.smell); // "banana"
```

当我们 mutate 一个共享的 prototype，将会造成原型链污染。

## Service Worker

### Getting Started

Main script:

```js
var worker = new Worker("worker.js");
```

worker.js:

```js
worker.postMessage();
```

### 通信 Hello World

Main script:

```js
var worker = new Worker("worker.js");

worker.addEventListener(
  "message",
  function (e) {
    console.log("Worker said: ", e.data);
  },
  false
);

worker.postMessage("Hello World"); // Send data to our worker.
```

worker.js:

```js
self.addEventListener(
  "message",
  function (e) {
    self.postMessage(e.data);
  },
  false
);
```

### 更复杂点的例子

Main script:

```html
<button onclick="sayHI()">Say HI</button>
<button onclick="unknownCmd()">Send unknown command</button>
<button onclick="stop()">Stop worker</button>
<output id="result"></output>

<script>
  function sayHI() {
    worker.postMessage({ cmd: "start", msg: "Hi" });
  }

  function stop() {
    // worker.terminate() from this script would also stop the worker.
    worker.postMessage({ cmd: "stop", msg: "Bye" });
  }

  function unknownCmd() {
    worker.postMessage({ cmd: "foobard", msg: "???" });
  }

  var worker = new Worker("worker.js");

  worker.addEventListener(
    "message",
    function (e) {
      document.getElementById("result").textContent = e.data;
    },
    false
  );
</script>
```

worker.js:

```js
self.addEventListener(
  "message",
  function (e) {
    var data = e.data;
    switch (data.cmd) {
      case "start":
        self.postMessage("WORKER STARTED: " + data.msg);
        break;
      case "stop":
        self.postMessage(
          "WORKER STOPPED: " + data.msg + ". (buttons will no longer work)"
        );
        self.close(); // Terminates the worker.
        break;
      default:
        self.postMessage("Unknown command: " + data.msg);
    }
  },
  false
);
```
