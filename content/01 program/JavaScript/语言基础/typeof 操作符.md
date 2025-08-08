## 1. typeof 操作符概述

`typeof` 是 JavaScript 中的一个一元操作符，用于检测变量或表达式的数据类型。它返回一个表示操作数类型的字符串。

### 基本语法

```javascript
typeof operand
typeof(operand)  // 括号是可选的
```

## 2. typeof 的返回值

typeof 操作符可能返回以下 8 种字符串值：

|返回值|对应的数据类型|
|---|---|
|`"undefined"`|未定义|
|`"boolean"`|布尔值|
|`"number"`|数字|
|`"bigint"`|大整数|
|`"string"`|字符串|
|`"symbol"`|符号|
|`"function"`|函数|
|`"object"`|对象（包括null、数组等）|

## 3. 基本数据类型的 typeof 检测

### 3.1 undefined

```javascript
let a;
console.log(typeof a);        // "undefined"
console.log(typeof undefined); // "undefined"
```

### 3.2 boolean

```javascript
console.log(typeof true);     // "boolean"
console.log(typeof false);    // "boolean"
console.log(typeof Boolean(1)); // "boolean"
```

### 3.3 number

```javascript
console.log(typeof 42);       // "number"
console.log(typeof 3.14);     // "number"
console.log(typeof NaN);      // "number" ⚠️ 注意：NaN也是number类型
console.log(typeof Infinity); // "number"
```

### 3.4 string

```javascript
console.log(typeof "hello");  // "string"
console.log(typeof 'world');  // "string"
console.log(typeof `模板`);    // "string"
console.log(typeof String(123)); // "string"
```

### 3.5 bigint (ES2020新增)

```javascript
console.log(typeof 123n);     // "bigint"
console.log(typeof BigInt(123)); // "bigint"
```

### 3.6 symbol (ES6新增)

```javascript
console.log(typeof Symbol());     // "symbol"
console.log(typeof Symbol('id')); // "symbol"
```

## 4. 对象类型的 typeof 检测

### 4.1 普通对象

```javascript
console.log(typeof {});       // "object"
console.log(typeof {a: 1});   // "object"
console.log(typeof new Object()); // "object"
```

### 4.2 数组

```javascript
console.log(typeof []);       // "object" ⚠️ 数组也是object
console.log(typeof [1,2,3]);  // "object"
console.log(typeof new Array()); // "object"
```

### 4.3 null的特殊情况

```javascript
console.log(typeof null);     // "object" ⚠️ 这是JavaScript的历史bug
```

**重要提醒**：`typeof null` 返回 `"object"` 是JavaScript的一个著名bug，但由于历史兼容性原因一直保留至今。

### 4.4 Date对象

```javascript
console.log(typeof new Date()); // "object"
```

### 4.5 正则表达式

```javascript
console.log(typeof /abc/);      // "object"
console.log(typeof new RegExp('abc')); // "object"
```

## 5. 函数类型

```javascript
function myFunc() {}
const arrowFunc = () => {};
const asyncFunc = async () => {};

console.log(typeof myFunc);    // "function"
console.log(typeof arrowFunc); // "function"
console.log(typeof asyncFunc); // "function"
console.log(typeof console.log); // "function"
```

## 6. 包装对象与原始类型的区别

### 6.1 原始类型 vs 包装对象

```javascript
// 原始类型
console.log(typeof "hello");        // "string"
console.log(typeof 123);            // "number"
console.log(typeof true);           // "boolean"

// 包装对象
console.log(typeof new String("hello")); // "object"
console.log(typeof new Number(123));     // "object"
console.log(typeof new Boolean(true));   // "object"
```

### 6.2 实际应用中的差异

```javascript
let str1 = "hello";           // 原始类型
let str2 = new String("hello"); // 包装对象

console.log(str1 == str2);    // true（值相等）
console.log(str1 === str2);   // false（类型不同）
console.log(typeof str1);     // "string"
console.log(typeof str2);     // "object"
```

## 7. typeof 的实际应用场景

### 7.1 参数类型检查

```javascript
function processValue(value) {
    if (typeof value === 'string') {
        return value.toUpperCase();
    } else if (typeof value === 'number') {
        return value * 2;
    } else if (typeof value === 'function') {
        return value();
    } else {
        return 'Unknown type';
    }
}
```

### 7.2 检查变量是否已定义

```javascript
// 安全的未定义检查
if (typeof someVariable !== 'undefined') {
    console.log('变量已定义');
}

// 避免ReferenceError
if (typeof window !== 'undefined') {
    // 浏览器环境代码
}
```

### 7.3 特性检测

```javascript
// 检查某个API是否存在
if (typeof fetch !== 'undefined') {
    // 使用fetch API
} else {
    // 使用polyfill或其他方法
}
```

## 8. typeof 的局限性

### 8.1 无法区分对象类型

```javascript
console.log(typeof []);       // "object"
console.log(typeof {});       // "object"
console.log(typeof null);     // "object"
console.log(typeof new Date()); // "object"
```

### 8.2 更精确的类型检测方法

#### 使用 Object.prototype.toString

```javascript
function getType(obj) {
    return Object.prototype.toString.call(obj).slice(8, -1);
}

console.log(getType([]));        // "Array"
console.log(getType({}));        // "Object"
console.log(getType(null));      // "Null"
console.log(getType(new Date())); // "Date"
```

#### 使用 Array.isArray

```javascript
console.log(Array.isArray([]));  // true
console.log(Array.isArray({}));  // false
```

#### 使用 instanceof

```javascript
console.log([] instanceof Array);   // true
console.log({} instanceof Object);  // true
console.log(new Date() instanceof Date); // true
```

## 9. 最佳实践

### 9.1 类型检查建议

```javascript
// ✅ 推荐的类型检查方式
function checkTypes(value) {
    // 基本类型用typeof
    if (typeof value === 'string') return 'string';
    if (typeof value === 'number') return 'number';
    if (typeof value === 'boolean') return 'boolean';
    if (typeof value === 'function') return 'function';
    
    // null单独检查
    if (value === null) return 'null';
    
    // 数组用Array.isArray
    if (Array.isArray(value)) return 'array';
    
    // 其他对象类型
    if (typeof value === 'object') return 'object';
    
    return 'unknown';
}
```

### 9.2 常见错误避免

```javascript
// ❌ 错误：用typeof检查数组
if (typeof arr === 'object') {
    // 这也会匹配null和其他对象
}

// ✅ 正确：用Array.isArray检查数组
if (Array.isArray(arr)) {
    // 只匹配数组
}

// ❌ 错误：用typeof检查null
if (typeof value === 'object' && value !== null) {
    // 需要额外排除null
}

// ✅ 正确：直接检查null
if (value === null) {
    // 直接比较
}
```

## 10. 总结

- `typeof` 是检测JavaScript数据类型的基础操作符
- 对于基本类型（除null外）检测准确可靠
- 对于对象类型，只能返回 `"object"` 或 `"function"`
- `typeof null` 返回 `"object"` 是历史遗留问题
- 在实际开发中，通常需要结合其他方法进行更精确的类型检测
- 理解typeof的特点和局限性，选择合适的类型检测方法

[[Undefined 类型|下一篇： Undefined 类型]]
