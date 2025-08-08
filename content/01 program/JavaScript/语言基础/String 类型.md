JavaScript 的 String 类型用于表示和操作一系列字符。

## 1. 创建字符串

创建字符串有两种主要方式：字面量和构造函数。

|   |   |   |
|---|---|---|
|方法|示例|说明|
|**字符串字面量**|const str = "Hello, World!";|这是最常用、最推荐的方式。简单、高效。|
|**String() 函数**|const str = String(123);|作为函数调用时，它会将任何类型的值转换为字符串，这在类型转换时非常有用。|
|**new String() 构造函数**|const strObj = new String("Hello");|使用 new 关键字会创建一个字符串对象（object），而不是一个原始字符串（primitive string）。这在大多数情况下应该避免，因为它可能导致意外的行为（例如 new String("a") === new String("a") 的结果是 false）。|

**注意：** 原始字符串和字符串对象在行为上有所不同。在方法调用时，JavaScript 会自动将原始字符串包装成一个临时对象，以便调用方法，这个过程称为“自动装箱”（autoboxing）。

## 2. 字符串的不可变性 (Immutability)

这是 String 类型最核心和最重要的特性之一。

- **一旦字符串被创建，它的值就不能被改变**。
    
- 所有看起来像是修改字符串的方法（如 replace(), toUpperCase(), slice() 等），实际上都不会改变原始字符串。
    
- 它们会 **返回一个新的字符串**。
    

```js
let greeting = "Hello";
greeting.toUpperCase(); // 返回 "HELLO"，但 greeting 本身并未改变
console.log(greeting); // 输出 "Hello"

let newGreeting = greeting.toUpperCase(); // 需要将返回的新字符串赋值给一个新变量
console.log(newGreeting); // 输出 "HELLO"
```

这个特性使得字符串在作为函数参数传递或在多处使用时更加可预测和安全。

## 3. 常用属性和方法

### 3.1 属性

- length: 返回字符串中字符的数量（对于UTF-16编码）。
```js
console.log("JavaScript".length); // 输出 10
```

### 3.2 字符访问

- **charAt(index)**: 返回指定索引位置的字符。
    
- **charCodeAt(index)**: 返回指定索引位置字符的 UTF-16 编码值（一个 0 到 65535 之间的整数）。
    
- **括号表示法 []**: 从 ES5 开始，可以使用类似数组的方式访问字符。这是推荐的访问方式。

```js
const str = "Hello";
console.log(str.charAt(1)); // "e"
console.log(str[1]);        // "e"
```

**注意：** 字符串索引从 0 开始。如果使用括号表示法访问一个不存在的索引，会返回 undefined，而 charAt() 会返回一个空字符串 ""。

### 3.3 查找与匹配

- **indexOf(searchValue, fromIndex?)**: 返回指定子字符串首次出现的索引，如果未找到则返回 -1。可以提供一个可选的起始搜索位置。
    
- **lastIndexOf(searchValue, fromIndex?)**: 与 indexOf 类似，但从字符串末尾向前搜索。
    
- **includes(searchValue, fromIndex?)**: 判断是否包含指定子字符串，返回 true 或 false。
    
- **startsWith(searchValue, fromIndex?)**: 判断是否以指定子字符串开头，返回 true 或 false。
    
- **endsWith(searchValue, length?)**: 判断是否以指定子字符串结尾，返回 true 或 false。
    
- **search(regexp)**: 使用正则表达式进行搜索，返回第一个匹配项的索引，未找到则返回 -1。
    
- **match(regexp)**: 根据正则表达式进行匹配，返回一个包含匹配结果的数组，如果没有匹配则返回 null。
    

### 3.4 提取与截取

- **slice(startIndex, endIndex?)**: 提取字符串的一部分并返回一个新字符串。支持负数索引，表示从字符串末尾开始计算。
    
- **substring(startIndex, endIndex?)**: 与 slice 类似，但不支持负数索引，并且它会自动将较小的索引作为起始点。
    
- **substr(startIndex, length?)**: (已废弃，不推荐使用) 从指定索引开始，提取指定长度的子字符串。
    

**slice vs substring 的区别:**
```js
let text = "Hello World";
text.slice(1, 5); // "ello"
text.substring(1, 5); // "ello"

text.slice(-5); // "World" (从倒数第5个字符到末尾)
text.substring(-5); // "Hello World" (负数索引被当作0)

text.slice(5, 1); // "" (起始索引大于结束索引)
text.substring(5, 1); // "ello" (自动交换索引 1 和 5)
```

**最佳实践：** 优先使用 slice()，它的行为更直观和可预测。

### 3.5 转换与操作

- **replace(searchValue, newValue)**: 替换匹配的子字符串。searchValue 可以是字符串或正则表达式。默认只替换第一个匹配项，要替换所有匹配项需使用正则表达式的全局标志 g。
    
- **replaceAll(searchValue, newValue)**: 替换所有匹配的子字符串。searchValue 可以是字符串或正则表达式（必须带全局标志 g）。
    
- **toLowerCase() / toUpperCase()**: 返回转换为小写或大写的新字符串。
    
- **trim() / trimStart() / trimEnd()**: 移除字符串两端、开头或结尾的空白字符。
    
- **concat(...strings)**: 连接一个或多个字符串，并返回新的字符串。通常使用加号 + 或模板字面量更方便。
    
- **split(separator, limit?)**: 使用指定的分隔符将字符串分割成一个字符串数组。
    
- **padStart(targetLength, padString?) / padEnd(targetLength, padString?)**: 在字符串的开头或结尾填充指定字符，直到达到目标长度。
    

## 4. 模板字面量 (Template Literals) (ES6+)

模板字面量是 ES6 引入的强大功能，它提供了创建字符串的增强语法。

- **使用反引号 `** 包裹字符串。
    
- **嵌入表达式**: 可以使用 ${expression} 语法轻松地在字符串中嵌入变量和表达式。
    
- **多行字符串**: 可以直接创建跨越多行的字符串，而无需使用 \n。
```js
const name = "Alice";
const score = 95;

// 旧的方式
const messageOld = "Hello, " + name + "!\nYour score is " + score + ".";

// 使用模板字面量
const messageNew = `Hello, ${name}!
Your score is ${score}.`;

console.log(messageNew);
/*
输出:
Hello, Alice!
Your score is 95.
*/
```

## 5. 处理 Unicode 和特殊字符

JavaScript 字符串使用 UTF-16 编码。对于大多数常见字符（位于基本多文种平面 BMP 中），length 属性和字符访问方法工作正常。

但是，对于超出 U+FFFF 的字符（如很多表情符号 "😊"），它们由两个代理对（surrogate pair）表示，这会导致一些问题：

```js
let emoji = "😊";
console.log(emoji.length); // 2
console.log(emoji.charAt(0)); // "\uD83D" (代理对的前半部分)
console.log(emoji.charAt(1)); // "\uDE0A" (代理对的后半部分)
```

**解决方案 (ES6+)**:

- 使用 for...of 循环可以正确地迭代这些字符。
    
- 使用 Array.from() 可以将字符串正确地转换为字符数组。
    

```js
const emoji = "😊";

// 正确的迭代
for (const char of emoji) {
    console.log(char); // "😊"
}

// 正确地获取字符数组
const chars = Array.from(emoji);
console.log(chars.length); // 1
console.log(chars); // ["😊"]
```

## 6. 性能和注意事项

- **字符串连接**: 对于大量的字符串连接操作，使用 + 或 += 在现代 JavaScript 引擎中已经非常高效。在旧的浏览器环境中，将字符串放入数组然后调用 array.join('') 可能会更快，但现在这种优化通常已非必要。模板字面量是目前可读性和性能俱佳的选择。
    
- **避免创建不必要的字符串**: 由于字符串的不可变性，每次“修改”都会创建新字符串。在循环中进行大量字符串操作时，要注意内存消耗。
    
- **正则表达式的威力**: 对于复杂的查找和替换任务，正则表达式 RegExp 是 String 方法的强大搭档，善用它可以极大地简化代码。

[[Symbol 类型|下一篇： Symbol 类型]]
