# 目录
+ [八皇后问题-回溯法](#八皇后问题-回溯法)
+ [暴力匹配-暴风算法(BFSearch)](#暴力匹配-暴风算法(BFSearch))
+ [猜字谜](#猜字谜)
+ [两数之和](#两数之和)
+ [冒泡排序](#冒泡排序)
+ [删除字符串中的所有相邻重复项](#删除字符串中的所有相邻重复项)
+ [托普利茨矩阵](#托普利茨矩阵)
+ [旋转字符串](#旋转字符串)
+ [杨辉三角II](#杨辉三角II)

## [八皇后问题-回溯法](https://leetcode-cn.com/problems/n-queens/)
```js
var queenPlace = []
var count = 0
var printStr = ''

function printQueen() {
	for (var i = 0; i < 8; i++) {
		for (var j = 0; j < 8; j++) {
			if (queenPlace[i] == j) {
				printStr += 'Q '
			} else {
				printStr += '* '
			}
		}
		printStr += '\n'
	}
	console.log(printStr)
	console.log(`----count:${++count}----\n`)
}

// 判断行列位置是否合适
function isOk(row, col) {
	var leftUp = col - 1			// 落点皇后的左上对角线
	var rightUp = col + 1			// 落点皇后的右上对角线

	for (var i = row - 1; i >= 0; i--) {
		if (queenPlace[i] == col) return false				// 同列上的格子有皇后
		if (leftUp >= 0) {
			if (queenPlace[i] == leftUp) return false		// 左上对角线有皇后
		}
		if (rightUp < 8) {
			if (queenPlace[i] == rightUp) return false		// 右上对角线有皇后
		}
		--leftUp
		++rightUp
	}
	return true
}

function eightQueen(row) {
	if (row == 8) {
		printQueen()
		return
	}
	for (var col = 0; col < 8; col++) {
		if (isOk(row, col)) {
			queenPlace[row] = col
			eightQueen(row + 1)
		}
	}
}

eightQueen(0)
```

## [暴力匹配-暴风算法(BFSearch)](https://www.geekxh.com/1.3.%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%B3%BB%E5%88%97/306.html#_01%E3%80%81%E5%9B%BE%E8%A7%A3%E5%88%86%E6%9E%90)
```js
// 理论时间复杂度为 O(m*n), m为目标串, n为模式串，实际效率会更高

/**
 * @param {[type]} haystack 目标串
 * @param {[type]} needle   模式串
 */
function BFSearch(haystack, needle) {
	var l1 = haystack.length
	var l2 = needle.length
	var i = 0, j = 0

	while (i < l1 && j < l2) {
		if (haystack[i] === needle[j]) {
			i++
			j++
		} else {
			i -= j - 1
			j = 0
		}
	}
	if (j === l2) {
		return i - j
	}
	return -1
}

var str1 = 'ABCABDABCEABD'
var str2 = 'ABCE'
BFSearch(str1, str2)
```

## [猜字谜](https://leetcode-cn.com/problems/number-of-valid-words-for-each-puzzle/)
```js
function isPuzzle(puzzle, word) {
	const puzzleFirstLetter = puzzle[0];

	if (!word.includes(puzzleFirstLetter)) return false;

	for (let i = 0; i < word.length; i++) {
		if (!puzzle.includes(word[i])) return false;
	}

	return true;
}

function findNumOfValidWords(words, puzzles) {
	let result = new Array(puzzles.length).fill(0);
	for (let i = 0; i < puzzles.length; i++) {
		const puzzle = puzzles[i];
		for (let j = 0; j < words.length; j++) {
			const word = words[j];
			if (isPuzzle(puzzle, word)) {
				result[i]++;
			}
		}
	}
	return result;
}

// 测试 findNumOfValidWords
const words = ["aaaa","asas","able","ability","actt","actor","access"];
const puzzles = ["aboveyz","abrodyz","abslute","absoryz","actresz","gaswxyz"];

const res = findNumOfValidWords(words, puzzles);
console.log(res);
```

## [两数之和](https://leetcode-cn.com/problems/two-sum/)
```js
// 实现一 时间复杂度： O(n^2)
function twoNums(arr, target) {
	for (let i = 0; i < arr.length; i++) {
		let curr = arr[i];
		for (let j = i + 1; j < arr.length; j++) {
			let next = arr[j];
			if (curr + next == target) {
				return [i, j];
			}
		}
	}
	return [];
}

// 实现二 使用哈希表 时间复杂度： O(n)
function twoNums2(arr, target) {
	const map = new Map();
	for (let i = 0; i < arr.length; i++) {
		if (map.has(target - arr[i])) {
			return [map.get(target - arr[i]), i];
		}
		map.set(arr[i], i);
	}
	return [];
}

const arr = [1,2,3,4,5,6,7,8];
const target = 6;
const res = twoNums2(arr, target);
console.log(res);
```

## 冒泡排序
```js
function bubbleSort(arr) {
	var len = arr.length
	for (var i = 0; i < len - 1; i++) {
		for (var j = 0; j < len - 1 - i; j++) {
			if (arr[j] > arr[j + 1]) {
				var temp = arr[j + 1]
				arr[j + 1] = arr[j]
				arr[j] = temp
			}
		}
	}
	return arr
}
```

## [删除字符串中的所有相邻重复项](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/)
```js
// 利用栈
function removeDuplicates(str) {
	const stk = [];
	for (const ch of str) {
		if (stk.length && stk[stk.length - 1] === ch) {
			stk.pop();
		} else {
			stk.push(ch);
		}
	}
	return stk.join('');
}

const str = "abbaca";
const res = removeDuplicates(str);
console.log(res);
```

## [托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)
```js
function isToeplitzMatrix(matrix) {
	const m = matrix.length, n = matrix[0].length;
	for (let i = 1; i < m; i++) {
		for (let j = 1; j < n; j++) {
			if (matrix[i][j] != matrix[i - 1][j - 1]) {
				return false;
			}
		}
	}
	return true;
}

const matrix = [
	[1,2,3,4], 
	[5,1,2,3], 
	[9,5,1,2]
];
console.log(isToeplitzMatrix(matrix));
```

## [旋转字符串](https://leetcode-cn.com/problems/rotate-string/)
```js
function rotateString(A, B) {
 	if (!A && !B) {
 		return true
 	}
 	const len = A.length
 	for (let i = 0; i < len; i++) {
 		const first = A.substring(0, 1)
 		const last = A.substring(1, len)
 		A = last + first
 		if (A === B) {
 			return true
 		}
 	}
 	return false
}

// 无论A怎么旋转，A + A 为所有可以通过旋转操作从A得到的字符串
function rotateString2(A, B) {
	return A.length === B.length && (A + A).includes(B)
}

const a = 'abcde'
const b = 'cdeab'

const res = rotateString(a, b)
console.log(res)
```

## [杨辉三角II](https://leetcode-cn.com/problems/pascals-triangle-ii/)
```js
// 返回杨辉三角的第 k 行
// 输入： 3
// 输出： [1,3,3,1]
// 首先求杨辉三角，然后返回行
function getRow(rowIndex) {
	const C = new Array(rowIndex + 1).fill(0);
	for (let i = 0; i <= rowIndex; i++) {
		C[i] = new Array(i + 1).fill(0);
		C[i][0] = C[i][i] = 1;		// 首尾都是1
		// 从第3行开始，中间数字等于上一行的左右两数之和
		for (let j = 1; j < i; j++) {
			C[i][j] = C[i - 1][j - 1] + C[i - 1][j];
		}
	}
	return C[rowIndex];
}

// 优化
// 对第 i+1i+1 行的计算仅用到了第 ii 行的数据
// 使用滚动数组的思想优化空间复杂度
function getRow2(rowIndex) {
	let pre = [], cur = [];
	for (let i = 0; i <= rowIndex; i++) {
		cur = new Array(i + 1).fill(0);
		cur[0] = cur[i] = 1;
		for (let j = 1; j < i; j++) {
			cur[j] = pre[j - 1] + pre[j];
		}
		pre = cur;
	}
	return pre;
}

// 进一步优化
// 只用一个数组
// 倒着计算当前行
function getRow3(rowIndex) {
	const row = new Array(rowIndex + 1).fill(0);
	row[0] = 1;
	for (let i = 0; i <= rowIndex; i++) {
		for (let j = i; j > 0; --j) {
			row[j] += row[j - 1];
		}
	}
	return row;
}

const res = getRow3(3);
console.log(res);
```