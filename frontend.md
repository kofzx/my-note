## mutate

mutate基本上就是change的意思，之所以叫mutate，是因为它有“危险”的意思，警示你要格外小心。

## const 

const防止变量重新分配，但对象的mutation是制止不了的。网上对于const的实用性一直都是一个备受争议的话题，有的人说禁止let，直接始终用const;也有人说应该信任程序员重新分配自己的变量。

## Prototypes
+ 当找到了对象自身的属性，将停止搜索原型链。
+ hasOwnProperty只会查找对象自身属性，不会搜索原型链。
+ 原型链污染
```js
let obj = {};
obj.__proto__.smell = 'banana';

let puzzle1 = {
	__proto__: obj
};
let puzzle2 = {
	__proto__: obj
};
console.log(puzzle1.smell);	// "banana"
console.log(puzzle2.smell);	// "banana"
```
当我们mutate一个共享的prototype，将会造成原型链污染。