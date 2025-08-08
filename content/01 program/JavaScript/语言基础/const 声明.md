`const`与`let`基本相同，主要的区别是：
- `const`在声明变量时必须要**初始化**
- 变量的值在声明后不能修改

```js
const age = 26;
age = 33;  // Error: 常量不能赋值

// 不允许重复声明
const name = 'Alice';
const name = 'Bob';  // Error!

// 作用域也是块
const name = 'Bob';
if(true) {
	const name = 'John';
}
console.log(name); // 'Bob'
```

`const`声明的限制只适用于引用（变量的初始地址），如果引用的是对象或数组，那么是可以修改其中的值的，因为它的初始地址没变，只是在后面又加了数据。

还有一些常见的用法：
```js
for(const key in {a: 10, b: 20}){
	console.log(key);
}
// a, b

for(const val of [1,2,3,4]) {
	console.log(val);
}
// 1，2，3，4
```

[[声明风格及最佳实践|下一篇： 声明风格及最佳实践]]
