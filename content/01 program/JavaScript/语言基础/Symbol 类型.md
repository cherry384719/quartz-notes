`Symbol` 是 ES6 引入的一种原始数据类型（primitive type），用于创建唯一的标识符。

## 一、基本概念

```js
 const sym1 = Symbol();  
 const sym2 = Symbol();  
 console.log(sym1 === sym2); // false
```

- `Symbol()` 每次调用都会生成一个唯一的值。
  
- 不可通过 `new Symbol()` 创建 —— 会抛出 TypeError。

## 二、使用场景

### 1. 作为对象属性的键（key）

```js
 const mySymbol = Symbol("desc");  
 const obj = {  
   [mySymbol]: "value"  
 };  
 console.log(obj[mySymbol]); // "value"
```

- 可用于定义“私有”属性，避免属性名冲突。

### 2. 避免属性名冲突（用于第三方库或混入场景）

```js
 const key = Symbol("uniqueKey");  
 ​  
 class MyClass {  
   constructor() {  
     this[key] = "private";  
   }  
 }
```

### 3. 枚举属性时不会被 `for...in` / `Object.keys()` 遍历

```js
 const s = Symbol("hidden");  
 const obj = { [s]: 42, visible: 100 };  
 ​  
 console.log(Object.keys(obj));      // ["visible"]  
 console.log(Object.getOwnPropertyNames(obj)); // ["visible"]  
 console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(hidden)]
```


## 三、`Symbol.for()` 与 `Symbol.keyFor()`

```js
 const a = Symbol.for("shared");  
 const b = Symbol.for("shared");  
 console.log(a === b); // true
```

- `Symbol.for()` 会在全局 Symbol 注册表中查找或创建 symbol。
  
- `Symbol.keyFor()` 可反查 key：


```js
 const sym = Symbol.for("example");  
 console.log(Symbol.keyFor(sym)); // "example"
```

> ✅ `Symbol()` 创建的是局部唯一，`Symbol.for()` 创建的是全局唯一。


## 四、内置 Symbol（Well-known Symbols）

ES6 提供了一些内置的 Symbol，用于修改 JS 的默认行为：

|Symbol 名称|用途说明|
|---|---|
|`Symbol.iterator`|使对象可迭代（for...of）|
|`Symbol.asyncIterator`|异步迭代器支持|
|`Symbol.toPrimitive`|对象转原始值时调用|
|`Symbol.toStringTag`|自定义 `Object.prototype.toString.call()` 的结果|
|`Symbol.hasInstance`|控制 `instanceof` 行为|
|`Symbol.isConcatSpreadable`|控制数组合并时是否展开|
|`Symbol.match`|被 `String.prototype.match` 调用时的行为|
|`Symbol.replace`|被 `String.prototype.replace` 调用|
|`Symbol.search`|被 `String.prototype.search` 调用|
|`Symbol.split`|被 `String.prototype.split` 调用|
|`Symbol.species`|控制派生对象返回的构造函数|
|`Symbol.unscopables`|控制 with 作用域中排除的属性|

### 示例：实现自定义可迭代对象

```js
 const obj = {  
   *[Symbol.iterator]() {  
     yield 1;  
     yield 2;  
     yield 3;  
   }  
 };  
 for (const val of obj) {  
   console.log(val); // 1, 2, 3  
 }
```


## 五、注意事项 ⚠️

1. **Symbol 不是字符串**

```js
     const sym = Symbol("desc");  
     console.log(typeof sym); // "symbol"
```
2. **不能与其他类型隐式转换**

```js
const sym = Symbol("desc");  
const str = "value" + sym; // ❌ TypeError: Cannot convert a Symbol value to a string
```

    👉 正确写法：
    
    const str = "value" + String(sym); // ✅

3. **不能 JSON 序列化**
   
```js
const obj = { key: Symbol("desc") };  
console.log(JSON.stringify(obj)); // "{}"
```

4. **适合作为私有属性或常量枚举的替代方案**
   
```js
const STATUS = {  
  READY: Symbol("ready"),  
  DONE: Symbol("done"),  
  ERROR: Symbol("error"),  
};
```


---

## 六、实践小技巧 💡

- 创建私有变量：不暴露 Symbol 就无法访问该属性。
  
- 实现状态枚举：比字符串常量更安全，防止冲突。
  
- 配合 `Object.getOwnPropertySymbols` 实现 Symbol 属性的获取。
  

---

## 七、总结

|特性|说明|
|---|---|
|唯一性|每个 Symbol 值都是唯一的|
|隐私性|不会出现在常规枚举中|
|安全性|适合避免属性冲突|
|灵活性|可用于元编程和高级对象行为自定义|


[[Object 类型|下一篇： Object 类型]]
