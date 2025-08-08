undefined 类型只有一个值 undefined。它表示一个变量已被声明，但还从来没有被赋值。

- **出现场景**:

	1. 一个变量被声明了但没有初始化（赋值）。
	    
	2. 尝试访问一个对象上不存在的属性。
	    
	3. 一个函数没有明确的 return 语句，或者 return 后面没有值。
- 示例：
```js
let user;
console.log(user); // 输出: undefined

const car = { brand: 'Toyota' };
console.log(car.model); // 输出: undefined

function doNothing() {
  // 没有 return 语句
}
console.log(doNothing()); // 输出: undefined
```

[[Null 类型|下一篇： Null 类型]]
