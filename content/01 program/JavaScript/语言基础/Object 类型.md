在 JavaScript 中，对象（Object）是一种复合数据类型，是所有复杂数据结构的基础。它是一个无序的键值对（key-value pair）集合，其中键是字符串或 `Symbol`，值可以是任何数据类型，包括其他对象。深入理解 Object 对于掌握 JavaScript至关重要。

## 1. 创建对象

有多种方式可以创建对象，最常用的是对象字面量。

| 方法 | 示例 | 说明 |
| --- | --- | --- |
| **对象字面量 (Object Literal)** | `const obj = { key: 'value' };` | 这是最常用、最简洁、性能最好的方式。强烈推荐。 |
| **`new Object()` 构造函数** | `const obj = new Object();` | 功能上与对象字面量 `{} `等价，但写法更冗长。 |
| **`Object.create()`** | `const obj = Object.create(proto);` | 创建一个新对象，并将其原型 (`[[Prototype]]`) 设置为指定的 `proto` 对象。这在实现继承时非常有用。 |

**ES6+ 增强的对象字面量语法:**

*   **属性值简写**: 如果属性名与持有值的变量名相同，可以省略冒号和值。
*   **方法简写**: 在对象中定义函数时可以省略 `: function`。
*   **计算属性名**: 可以用方括号 `[]` 动态设置属性名。

```javascript
const name = 'Alice';
const age = 30;

const person = {
    // 1. 属性值简写
    name,
    age,

    // 2. 方法简写
    greet() {
        console.log(`Hello, I am ${this.name}.`);
    },

    // 3. 计算属性名
    ['skill_' + Math.random()]: 'JavaScript'
};

console.log(person);
// { name: 'Alice', age: 30, greet: [Function: greet], skill_0.123...: 'JavaScript' }
person.greet(); // "Hello, I am Alice."
```

## 2. 属性的操作 (增删改查)

### 2.1 访问 (查)

*   **点表示法 (Dot Notation)**: `object.propertyName`
    *   简单直观，是首选方式。
    *   属性名必须是有效的标识符（不能以数字开头，不能包含特殊字符等）。
*   **方括号表示法 (Bracket Notation)**: `object['propertyName']`
    *   更灵活，方括号内可以是任何返回字符串或 Symbol 的表达式。
    *   当属性名是无效标识符或动态变量时，必须使用此方法。

```javascript
const user = {
    name: 'Bob',
    'home-address': '123 Main St'
};

console.log(user.name); // 'Bob'
// console.log(user.home-address); // SyntaxError

console.log(user['home-address']); // '123 Main St'

const key = 'name';
console.log(user[key]); // 'Bob' - 动态访问
```

### 2.2 添加与修改 (增/改)

直接对属性赋值即可，如果属性已存在则修改其值，如果不存在则创建该属性。

```javascript
const car = {
    brand: 'Ford'
};

// 修改已有属性
car.brand = 'Toyota';

// 添加新属性
car.year = 2023;
car['color'] = 'blue';

console.log(car); // { brand: 'Toyota', year: 2023, color: 'blue' }
```

### 2.3 删除 (删)

使用 `delete` 操作符可以删除对象的某个属性。

```javascript
const book = {
    title: 'The Hobbit',
    author: 'J.R.R. Tolkien'
};

delete book.author;
console.log(book); // { title: 'The Hobbit' }
```

**注意**: `delete` 只能删除对象自身的属性，无法删除其原型链上的属性。

## 3. 常用 Object 静态方法

这些是挂载在 `Object` 构造函数上的工具方法，非常实用。

*   **`Object.keys(obj)`**: 返回一个由对象**自身可枚举**的**字符串属性名**组成的数组。
*   **`Object.values(obj)`**: 返回一个由对象**自身可枚举**的**属性值**组成的数组。
*   **`Object.entries(obj)`**: 返回一个由对象**自身可枚举**的 `[key, value]` 键值对数组组成的数组。这在与 `Map` 或 `for...of` 结合使用时特别方便。
*   **`Object.assign(target, ...sources)`**: 将一个或多个源对象 (`sources`) 的所有**可枚举**属性复制到目标对象 (`target`)，并返回修改后的 `target`。这是一个浅拷贝。
*   **`Object.freeze(obj)`**: 冻结一个对象。冻结后的对象不能添加、删除或修改属性。
*   **`Object.seal(obj)`**: 封闭一个对象。封闭后的对象不能添加或删除属性，但可以修改现有属性的值。
*   **`Object.hasOwn(obj, prop)`**: (ES2022+) 判断属性是否为对象**自身**的属性（而不是继承来的），比 `obj.hasOwnProperty()` 更推荐。

```javascript
const product = {
    id: 1,
    name: 'Laptop',
    price: 1200
};

console.log(Object.keys(product));   // ['id', 'name', 'price']
console.log(Object.values(product)); // [1, 'Laptop', 1200]
console.log(Object.entries(product)); // [['id', 1], ['name', 'Laptop'], ['price', 1200]]

const defaults = { stock: 0 };
const item = Object.assign({}, defaults, product); // 合并对象
console.log(item); // { stock: 0, id: 1, name: 'Laptop', price: 1200 }
```

## 4. 对象遍历

遍历对象属性是常见操作，有多种方式，各有不同。

| 方法 | 是否遍历原型链属性 | 是否遍历 Symbol 属性 | 是否遍历不可枚举属性 | 推荐场景 |
| :--- | :--- | :--- | :--- | :--- |
| **`for...in`** | **是** | 否 | 否 | 不推荐直接用于遍历自身属性，需配合 `hasOwnProperty` 或 `Object.hasOwn` |
| **`Object.keys().forEach()`** | 否 | 否 | 否 | 最常用，只遍历自身可枚举的字符串键 |
| **`Object.values().forEach()`** | 否 | 否 | 否 | 当你只关心值的时候 |
| **`Object.entries().forEach()`** | 否 | 否 | 否 | 当你同时需要键和值的时候 |
| **`Reflect.ownKeys().forEach()`** | 否 | **是** | **是** | 需要遍历所有自身属性，包括 Symbol 和不可枚举属性时 |

**`for...in` 的陷阱与正确用法**:

`for...in` 会遍历对象自身及其原型链上所有可枚举的属性，这可能导致意外行为。

```javascript
// 不安全的 for...in
for (const key in myObject) {
    console.log(myObject[key]); // 可能会打印出继承来的属性
}

// 安全的 for...in (推荐模式)
for (const key in myObject) {
    if (Object.hasOwn(myObject, key)) { // 只处理自身属性
        console.log(myObject[key]);
    }
}
```

**最佳实践**: 优先使用 `Object.keys`, `Object.values`, `Object.entries` 配合数组方法（如 `forEach`, `map`）进行遍历，意图更清晰，也更安全。

## 5. 高级概念

### 5.1 `this` 关键字

`this` 的值在 JavaScript 中是动态的，取决于函数的调用方式，而不是定义位置。在对象方法中，`this` 通常指向调用该方法的对象。

```javascript
const userProfile = {
    name: 'Charlie',
    getName: function() {
        return this.name;
    }
};

console.log(userProfile.getName()); // 'Charlie'，this 指向 userProfile

const standaloneGetName = userProfile.getName;
console.log(standaloneGetName()); // undefined (在非严格模式下) 或 TypeError (严格模式下)，因为 this 指向全局对象或 undefined
```

**箭头函数中的 `this`**: 箭头函数没有自己的 `this` 绑定，它会捕获其定义时所在上下文的 `this` 值。

### 5.2 原型与原型链 (Prototype & Prototype Chain)

每个 JavaScript 对象都有一个私有属性 `[[Prototype]]`（可以通过 `Object.getPrototypeOf(obj)` 访问），它指向另一个对象或 `null`。这个被指向的对象就是“原型”。

当你试图访问一个对象的属性时，如果在该对象自身上找不到，JavaScript 引擎会沿着原型链向上查找，直到找到该属性或到达链的末端 (`null`)。这是 JavaScript 实现继承的基础。

### 5.3 属性描述符 (Property Descriptors)

对象的每个属性都有一组内部特性，称为属性描述符。

*   `value`: 属性的值。
*   `writable`: `true` 表示属性值可以被修改。
*   `enumerable`: `true` 表示属性可以被 `for...in` 或 `Object.keys()` 等枚举。
*   `configurable`: `true` 表示属性可以被删除，其描述符也可以被修改。

使用 `Object.getOwnPropertyDescriptor(obj, prop)` 查看描述符，使用 `Object.defineProperty(obj, prop, descriptor)` 定义或修改属性及其描述符。

## 6. 注意事项与总结

1.  **首选字面量**: 始终优先使用 `{}` 创建对象。
2.  **引用类型**: 对象是引用类型。将一个对象赋值给另一个变量，实际上是复制了引用（内存地址），而不是对象本身。修改其中一个会影响另一个。若要复制对象，请考虑浅拷贝（`Object.assign` 或展开语法 `{...obj}`）或深拷贝。
3.  **键的类型**: 对象键会被隐式转换为字符串。`{ 1: 'a', '1': 'b' }` 最终只会有一个属性 `'1': 'b'`。
4.  **遍历的确定性**: 从 ES2015 开始，对于常规属性的遍历顺序有了一定的规定（整数键按升序，字符串键按插入顺序），但仍不应完全依赖于此顺序进行编程。
5.  **`hasOwnProperty` vs `Object.hasOwn`**: 优先使用 `Object.hasOwn(obj, prop)`，因为它更安全，能避免原型链上可能存在的同名方法带来的问题。
6.  **理解浅拷贝与深拷贝**: `Object.assign()` 和展开语法 `{...obj}` 都执行的是浅拷贝。如果对象的属性值还是一个对象，那么只复制了那个内层对象的引用。

---

[[操作符|下一篇： 操作符]]