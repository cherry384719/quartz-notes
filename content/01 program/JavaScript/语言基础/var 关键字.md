`var` 可以定义变量，类似C++中的`int`、`bool`等，比如：

```js
var message；
```
这行代码定义了一个变量`message`，可以用它来存储**任意**类型的值。（不初始化条件下，变量会保存一个特殊值undefined）

也可以在声明时就直接赋值：
```js
var message = 'hi';
```
此时`message`就成了一个保存字符串值`'hi'`的变量，然后可以改变保存的值，甚至可以直接改变值的类型：
```js
var message = 'hi'; // 赋值变量为字符串型
message = 100; // 变为了number类型，合法，但不推荐
```

### 1. var 声明作用域
使用`var`定义的变量会成为包含它的函数的局部变量，在函数执行完退出后，变量被销毁：
```js
function test() {
	var message = 'hi'; //局部变量
}
test();
console.log(message); //出错！
```

但，如果在创建变量时省略`var`操作符，可以创建全局变量：
```js
function test() {
	message = 'hi'; // 去掉了var，全局变量
}
test();
console.log(message); // 'hi'
```
只需要调用一次`test()`，就会定义这个全局变量，并且可以在函数外部访问它。

> **注意**：虽然可以通过省略`var`来定义全局变量，但是不推荐，很难维护代码，也容易造成困惑。在严格模式下，这种操作会报错。

如果需要定义多个变量，可以直接用逗号分隔每个变量，如：
```js
var message = 'hi',
	found = false,
	age = 18;
```
由于JavaScript属于弱语言，可以不必像C++一样将每种变量的类型(`int`,`string`,`char`,`bool`等) 写出来，它会自动识别数据类型。

### 2.var 声明提升
使用`var`时，下面的代码不会出错，因为`var`关键词声明的变量会自动提升到函数作用域顶部：
```js
function foo() {
	console.log(age);
	var age = 18;
}
foo();  //undefined
```

之所以不会报错，是因为代码等价于如下代码：
```js
function foo() {
	var age;  // 声明直接提到了这儿
	console.log(age);
	age = 18; // 然后进行了赋值
}
foo();  //undefined
```
这种直接提升到函数顶部的过程，是在函数顺序执行前就完成了。此外，反复多次用`var`声明同一个变量也没有问题：
```js
function foo() {
	var age = 18;
	var age = 28;
	var age = 38;
	
	console.log(age);
}
foo(); // 38
```

[[let 声明|下一篇： let 声明]]
