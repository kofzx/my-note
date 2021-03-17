## camelize
```js
function cached (fn) {
	const cache = Object.create(null);
	return function cachedFn (str) {
		const hit = cache[str];
		return hit || (cache[str] = fn(str));
	}
}

const camelizeRE = /-(\w)/g;
const camelize = cached((str) => {
	return str.replace(camelizeRE, (_, c) => c ? c.toUpperCase() : '');
});
```

## shallowEqual
```js
function shallowEqual(obj, newObj) {
	if (obj === newObj) {
		return true;
	}

	const objKeys = Object.keys(obj);
	const newObjKeys = Object.keys(newObj);

	if (objKeys.length !== newObjKeys.length) {
		return false;
	}

	return objKeys.every(key => {
		return newObjKeys[key] === obj[key];
	});
}
```

## mergeData
```js
function isNative (Ctor) {
	return typeof Ctor === 'function' && /native code/.test(Ctor.toString());
}

const hasSymbol =
	typeof Symbol !== 'undefined' && isNative(Symbol) &&
	typeof Reflect !== 'undefined' && isNative(Reflect.ownKeys);

const hasOwnProperty = Object.prototype.hasOwnProperty;
function hasOwn (obj, key) {
	return hasOwnProperty.call(obj, key);
}

const _toString = Object.prototype.toString;
function isPlainObject (obj) {
	return _toString.call(obj) === '[object Object]';
}

// 深拷贝合并两个对象
function mergeData (to, from) {
	if (!from) return to;
	let key, toVal, fromVal;

	const keys = hasSymbol
		? Reflect.ownKeys(from)
		: Object.keys(from);

	for (let i = 0; i < keys.length; i++) {
		key = keys[i];
		toVal = to[key];
		fromVal = from[key];
		if (!hasOwn(to, key)) {
			// set(to, key, fromVal);
			to[key] = fromVal;
		} else if (
			toVal !== fromVal &&
			isPlainObject(toVal) &&
			isPlainObject(fromVal)
		) {
			mergeData(toVal, fromVal);
		}
	}
	return to;
}
```

## replaceStr
```js
function replaceStr() {
	var args = Array.prototype.slice.call(arguments),
		target = args[0],
     	reg = new RegExp('\\{\\S+\\}', 'g');

    var matches = target.match(reg);

    for (var i = 1, len = args.length; i < len; i++) {
    	target = target.replace(matches[i - 1], args[i]);
    }
    return target;
}

// usage
var target = 'Tom has {num} flowers and {aaa} qq and {total} yuan.';
var res = replaceStr(target, 2, 100, 666);
console.log(res);	// Tom has 2 flowers and 100 qq and 666 yuan.
```