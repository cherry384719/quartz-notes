`let`与`var`作用差不多，但有重要的区别。最明显的是`let`声明的范围是==块作用域==，而`var`声明的范围是**函数作用域**。
```js
if(true) {
	var name = 'Bob';
	console.log(name); // 'Bob'
}
console.log(name); // 'Bob'


if(true) {
	let age = 26;
	console.log(age); // 26
}
console.log(age); // 报错：age没有定义
```
这里的`age`无法在`if`块外部使用，是因为它的作用于就只是在代码块中，也就是`{}`中。块作用域是函数作用域的子集。

`let`不允许在同一作用域下多次声明，这样会报错：
```js
var name;
var name;

let age;
let age;  // Error: 标识符age已声明过了
```
在多层作用域子下可以嵌套使用，系统会记录相应的标识符和块作用域：
```js
var name = 'Alice';
console.log(name);  // 'Alice'
if(true) {
	var name = 'Bob';
	console.log(name); // 'Bob'
}

let age = 30;
console.log(age); // 30
if(true) {
	let age = 40
	console.log(age); // 40
}
```

对声明的冗余报错不会区分`var`和`let`：
```js
var name;
let name; // Error!

let age;
var age; // Error!
```

### 1. 暂时性死区
`let`与`var`另一个重要的区别是：`let`声明的变量不会在作用域中被提升。
```js
//name会被提升
console.log(name);  // undefined
var name = 'Bob';

//age不会被提升
console.log(age);  // Error：age没有定义
let age = 18;
```
在`let`声明之前的执行瞬间叫做”暂时性死区“，在此阶段使用任何后面才定义的变量都会报错。

### 2.全局声明
与`var`不同，使用`let`在全局作用域中声明的变量不会像`var`一样成为window对象的属性。
```js
var name = 'Bob';
console.log(window.name); // 'Bob'

let age = 26;
console.log(window.age); // undefined
```

### 3. 条件声明
在使用 var 声明变量时，由于声明会被提升，JavaScript 引擎会自动将多余的声明在作用域顶部合并为一个声明。因为 let 的作用域被限定在块中，所以不可能检查前面是否已经使用 let 声明过同名变量并在没有声明的情况下声明它（条件声明）。

### 4. for 循环中的 let 声明
在`let`出现之前，使用 for 循环会泄露迭代变量到外部：
```js
for(var i = 0; i < 5; i++){
	//循环逻辑
}
console.log(i); // 5
```
使用`let`之后就不存在这个问题，因为迭代变量的作用域被限制在了 for 循环块内部：
```js
for(let i = 0; i < 5; i++){
	//循环逻辑
}
console.log(i); // Error：i没有定义
```
在使用`var`时还有一个常见的问题：
```js
for(var i = 0; i < 5; i++){
	setTimeout(() => console.log(i), 0);
}
//你可能认为输出：0，1，2，3，4
//但实际输出：5，5，5，5，5
```
因为定时器在执行时，i 已经退出循环变为了5，我画了一幅图来理解：
![image](https://github.com/cherry384719/picx-images-hosting/raw/master/image.58hop9x43a.webp)
所以有：
```js
for(let i = 0; i < 5; i++){
	setTimeout(() => console.log(i), 0);
}
// 输出 0,1,2,3,4
```
这种每次迭代都会声明一个新地址的行为适用于所以 for 循环，包括 for-in 和 for-of。


[[const 声明|下一篇： const 声明]]