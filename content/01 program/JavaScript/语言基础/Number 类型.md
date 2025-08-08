## 1. Number 类型基础

JavaScript 中的 Number 类型基于 IEEE 754 双精度 64 位浮点数标准，用于表示整数和浮点数。

### 1.1 Number 字面量

```javascript
// 整数
let int1 = 42;
let int2 = -17;
let int3 = 0;

// 浮点数
let float1 = 3.14;
let float2 = -2.5;
let float3 = 0.1;

// 科学计数法
let sci1 = 1e5;      // 100000
let sci2 = 1.23e-4;  // 0.000123
let sci3 = 5.67e+2;  // 567

// 不同进制
let binary = 0b1010;     // 二进制，值为 10
let octal = 0o755;       // 八进制，值为 493  
let hex = 0xFF;          // 十六进制，值为 255

console.log(typeof int1);    // "number"
console.log(typeof float1);  // "number"
console.log(typeof sci1);    // "number"
```

### 1.2 Number 构造函数

```javascript
// 作为函数调用（类型转换）
let num1 = Number("123");     // 123
let num2 = Number("3.14");    // 3.14
let num3 = Number(true);      // 1
let num4 = Number(false);     // 0

// 作为构造函数调用（创建Number对象）
let numObj = new Number(123); // Number对象
console.log(typeof numObj);   // "object"
console.log(numObj.valueOf()); // 123
```

## 2. 特殊的 Number 值

### 2.1 NaN (Not a Number)

```javascript
// 产生 NaN 的情况
console.log(0 / 0);           // NaN
console.log("hello" - 5);     // NaN
console.log(Math.sqrt(-1));   // NaN
console.log(parseInt("abc")); // NaN

// NaN 的特性
console.log(typeof NaN);      // "number" ⚠️ NaN也是number类型
console.log(NaN === NaN);     // false ⚠️ NaN不等于自身
console.log(NaN == NaN);      // false

// 检测 NaN
console.log(isNaN(NaN));         // true
console.log(Number.isNaN(NaN));  // true
console.log(isNaN("hello"));     // true ⚠️ 会先转换为数字
console.log(Number.isNaN("hello")); // false ⚠️ 更严格的检查

// 推荐的 NaN 检测方法
function isReallyNaN(value) {
    return value !== value; // 只有NaN不等于自身
}
console.log(isReallyNaN(NaN));    // true
console.log(isReallyNaN("hello")); // false
```

### 2.2 Infinity 和 -Infinity

```javascript
// 产生 Infinity 的情况
console.log(1 / 0);           // Infinity
console.log(-1 / 0);          // -Infinity
console.log(Number.MAX_VALUE * 2); // Infinity

// Infinity 的运算
console.log(Infinity + 1);    // Infinity
console.log(Infinity - 1);    // Infinity
console.log(Infinity * 2);    // Infinity
console.log(Infinity / 2);    // Infinity
console.log(Infinity / Infinity); // NaN

// 检测 Infinity
console.log(isFinite(123));      // true
console.log(isFinite(Infinity)); // false
console.log(Number.isFinite(123));      // true
console.log(Number.isFinite("123"));    // false ⚠️ 更严格
```

## 3. 数值转换

### 3.1 Number() 转换规则

```javascript
// 布尔值转换
console.log(Number(true));      // 1
console.log(Number(false));     // 0

// 字符串转换
console.log(Number("123"));     // 123
console.log(Number("123.45"));  // 123.45
console.log(Number(""));        // 0 ⚠️ 空字符串转为0
console.log(Number(" "));       // 0 ⚠️ 空格字符串转为0
console.log(Number("123abc"));  // NaN
console.log(Number("0x10"));    // 16 (十六进制)
console.log(Number("1e5"));     // 100000 (科学计数法)

// null 和 undefined
console.log(Number(null));      // 0 ⚠️
console.log(Number(undefined)); // NaN ⚠️

// 对象转换
console.log(Number({}));        // NaN
console.log(Number([]));        // 0 ⚠️ 空数组转为0
console.log(Number([5]));       // 5 ⚠️ 单元素数组
console.log(Number([1,2]));     // NaN
```

### 3.2 parseInt() 和 parseFloat()

```javascript
// parseInt() - 解析整数
console.log(parseInt("123"));      // 123
console.log(parseInt("123.45"));   // 123 (忽略小数部分)
console.log(parseInt("123abc"));   // 123 (忽略非数字部分)
console.log(parseInt("abc123"));   // NaN (开头不是数字)
console.log(parseInt("  123  "));  // 123 (忽略空格)

// parseInt() 的进制参数
console.log(parseInt("10", 2));    // 2 (二进制)
console.log(parseInt("10", 8));    // 8 (八进制)
console.log(parseInt("10", 16));   // 16 (十六进制)
console.log(parseInt("FF", 16));   // 255

// parseFloat() - 解析浮点数
console.log(parseFloat("123.45")); // 123.45
console.log(parseFloat("123.45.67")); // 123.45 (第二个点被忽略)
console.log(parseFloat("123abc"));  // 123
console.log(parseFloat("3.14e2"));  // 314 (支持科学计数法)
```

### 3.3 隐式类型转换

```javascript
// 算术运算中的转换
console.log("5" - 3);      // 2 (字符串转数字)
console.log("5" * 2);      // 10
console.log("10" / 2);     // 5
console.log("5" + 3);      // "53" ⚠️ 加号是字符串拼接

// 一元加号转换
console.log(+"123");       // 123
console.log(+true);        // 1
console.log(+false);       // 0
console.log(+null);        // 0
console.log(+undefined);   // NaN
console.log(+"");          // 0
```

## 4. 数字精度和浮点数陷阱

### 4.1 浮点数精度问题

```javascript
// 经典的浮点数精度问题
console.log(0.1 + 0.2);           // 0.30000000000000004 ⚠️
console.log(0.1 + 0.2 === 0.3);   // false ⚠️

// 更多精度问题示例
console.log(0.1 * 3);             // 0.30000000000000004
console.log(0.3 - 0.1);           // 0.19999999999999998
console.log(0.1 + 0.1 + 0.1);     // 0.30000000000000004
```

### 4.2 浮点数比较和处理

```javascript
// 错误的比较方式
function badEquals(a, b) {
    return a === b; // 对浮点数不可靠
}
console.log(badEquals(0.1 + 0.2, 0.3)); // false

// 正确的浮点数比较
function floatEquals(a, b, epsilon = 1e-10) {
    return Math.abs(a - b) < epsilon;
}
console.log(floatEquals(0.1 + 0.2, 0.3)); // true

// 使用 Number.EPSILON
function preciseEquals(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
}
console.log(preciseEquals(0.1 + 0.2, 0.3)); // true

// 四舍五入到指定小数位
function roundToPrecision(num, precision) {
    const factor = Math.pow(10, precision);
    return Math.round(num * factor) / factor;
}
console.log(roundToPrecision(0.1 + 0.2, 2)); // 0.3
```

### 4.3 整数的安全范围

```javascript
// JavaScript 安全整数范围
console.log(Number.MAX_SAFE_INTEGER);  // 9007199254740991 (2^53 - 1)
console.log(Number.MIN_SAFE_INTEGER);  // -9007199254740991

// 检查是否为安全整数
console.log(Number.isSafeInteger(123));                    // true
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER)); // true
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false

// 超出安全范围的问题
console.log(Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2); // true ⚠️
```

## 5. Number 对象的属性和方法

### 5.1 静态属性

```javascript
// 数值范围
console.log(Number.MAX_VALUE);         // 1.7976931348623157e+308
console.log(Number.MIN_VALUE);         // 5e-324 (最小正值)
console.log(Number.MAX_SAFE_INTEGER);  // 9007199254740991
console.log(Number.MIN_SAFE_INTEGER);  // -9007199254740991

// 特殊值
console.log(Number.POSITIVE_INFINITY); // Infinity
console.log(Number.NEGATIVE_INFINITY); // -Infinity
console.log(Number.NaN);               // NaN
console.log(Number.EPSILON);           // 2.220446049250313e-16
```

### 5.2 静态方法

```javascript
// 类型检查方法
console.log(Number.isInteger(123));    // true
console.log(Number.isInteger(123.0));  // true
console.log(Number.isInteger(123.5));  // false

console.log(Number.isFinite(123));     // true
console.log(Number.isFinite(Infinity)); // false
console.log(Number.isFinite("123"));   // false ⚠️ 不会转换类型

console.log(Number.isSafeInteger(123)); // true
console.log(Number.isSafeInteger(2**53)); // false

console.log(Number.isNaN(NaN));        // true
console.log(Number.isNaN("hello"));    // false ⚠️ 不会转换类型

// 解析方法
console.log(Number.parseInt("123px", 10));   // 123
console.log(Number.parseFloat("123.45px"));  // 123.45
```

### 5.3 实例方法

```javascript
let num = 123.456789;

// 格式化方法
console.log(num.toFixed(2));        // "123.46" (四舍五入到2位小数)
console.log(num.toExponential(2));  // "1.23e+2" (科学计数法)
console.log(num.toPrecision(5));    // "123.46" (总共5位有效数字)

// 进制转换
let num2 = 255;
console.log(num2.toString());       // "255"
console.log(num2.toString(2));      // "11111111" (二进制)
console.log(num2.toString(8));      // "377" (八进制)
console.log(num2.toString(16));     // "ff" (十六进制)

// 本地化格式
let num3 = 1234567.89;
console.log(num3.toLocaleString());       // "1,234,567.89" (根据地区格式)
console.log(num3.toLocaleString('zh-CN'));    // "1,234,567.89"
console.log(num3.toLocaleString('en-US', {
    style: 'currency',
    currency: 'USD'
})); // "$1,234,567.89"
```

## 6. 数学运算和方法

### 6.1 基本算术运算

```javascript
let a = 10, b = 3;

console.log(a + b);    // 13 (加法)
console.log(a - b);    // 7  (减法)
console.log(a * b);    // 30 (乘法)
console.log(a / b);    // 3.3333... (除法)
console.log(a % b);    // 1  (取模/余数)
console.log(a ** b);   // 1000 (幂运算，ES2016)

// 注意运算优先级
console.log(2 + 3 * 4);     // 14 (不是20)
console.log((2 + 3) * 4);   // 20
console.log(2 ** 3 ** 2);   // 512 (2^(3^2) = 2^9)
console.log((2 ** 3) ** 2); // 64  ((2^3)^2 = 8^2)
```

### 6.2 Math 对象常用方法

```javascript
// 舍入方法
console.log(Math.round(4.7));    // 5  (四舍五入)
console.log(Math.round(4.4));    // 4
console.log(Math.ceil(4.1));     // 5  (向上取整)
console.log(Math.floor(4.9));    // 4  (向下取整)
console.log(Math.trunc(4.9));    // 4  (截断小数部分，ES6)

// 绝对值和符号
console.log(Math.abs(-5));       // 5  (绝对值)
console.log(Math.sign(-5));      // -1 (符号：-1, 0, 1)
console.log(Math.sign(0));       // 0
console.log(Math.sign(5));       // 1

// 最值
console.log(Math.max(1, 5, 3));  // 5
console.log(Math.min(1, 5, 3));  // 1
console.log(Math.max(...[1,5,3])); // 5 (展开数组)

// 幂和根
console.log(Math.pow(2, 3));     // 8  (2的3次方)
console.log(Math.sqrt(9));       // 3  (平方根)
console.log(Math.cbrt(8));       // 2  (立方根，ES6)

// 随机数
console.log(Math.random());      // 0-1之间的随机数
console.log(Math.floor(Math.random() * 10)); // 0-9的随机整数
```

## 7. 实际应用场景

### 7.1 数值验证和处理

```javascript
// 验证是否为有效数字
function isValidNumber(value) {
    return typeof value === 'number' && 
           !isNaN(value) && 
           isFinite(value);
}

console.log(isValidNumber(123));      // true
console.log(isValidNumber(NaN));      // false
console.log(isValidNumber(Infinity)); // false
console.log(isValidNumber("123"));    // false

// 安全的数字转换
function safeNumberConvert(value, defaultValue = 0) {
    const num = Number(value);
    return isValidNumber(num) ? num : defaultValue;
}

console.log(safeNumberConvert("123"));    // 123
console.log(safeNumberConvert("abc"));    // 0
console.log(safeNumberConvert("abc", -1)); // -1
```

### 7.2 金额处理

```javascript
// 金额计算（避免浮点数精度问题）
class MoneyCalculator {
    // 转换为分（避免小数）
    static toCents(yuan) {
        return Math.round(yuan * 100);
    }
    
    // 转换为元
    static toYuan(cents) {
        return cents / 100;
    }
    
    // 金额加法
    static add(yuan1, yuan2) {
        const cents1 = this.toCents(yuan1);
        const cents2 = this.toCents(yuan2);
        return this.toYuan(cents1 + cents2);
    }
    
    // 金额减法
    static subtract(yuan1, yuan2) {
        const cents1 = this.toCents(yuan1);
        const cents2 = this.toCents(yuan2);
        return this.toYuan(cents1 - cents2);
    }
    
    // 格式化显示
    static format(yuan) {
        return `¥${yuan.toFixed(2)}`;
    }
}

console.log(MoneyCalculator.add(0.1, 0.2));     // 0.3
console.log(MoneyCalculator.format(123.456));   // "¥123.46"
```

### 7.3 随机数工具

```javascript
class RandomUtils {
    // 生成指定范围的随机整数
    static randomInt(min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }
    
    // 生成指定范围的随机浮点数
    static randomFloat(min, max, precision = 2) {
        const num = Math.random() * (max - min) + min;
        return parseFloat(num.toFixed(precision));
    }
    
    // 从数组中随机选择元素
    static randomChoice(array) {
        return array[this.randomInt(0, array.length - 1)];
    }
    
    // 生成随机ID
    static randomId(length = 8) {
        const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
        let result = '';
        for (let i = 0; i < length; i++) {
            result += this.randomChoice([...chars]);
        }
        return result;
    }
}

console.log(RandomUtils.randomInt(1, 10));           // 1-10的随机整数
console.log(RandomUtils.randomFloat(0, 1, 3));       // 0.123类似的随机数
console.log(RandomUtils.randomChoice(['a','b','c'])); // 随机选择一个
console.log(RandomUtils.randomId(12));               // 随机12位ID
```

### 7.4 数值格式化

```javascript
class NumberFormatter {
    // 千位分隔符
    static addCommas(num) {
        return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    }
    
    // 文件大小格式化
    static formatBytes(bytes, decimals = 2) {
        if (bytes === 0) return '0 B';
        
        const k = 1024;
        const sizes = ['B', 'KB', 'MB', 'GB', 'TB'];
        const i = Math.floor(Math.log(bytes) / Math.log(k));
        
        return parseFloat((bytes / Math.pow(k, i)).toFixed(decimals)) + ' ' + sizes[i];
    }
    
    // 百分比格式化
    static toPercent(decimal, precision = 1) {
        return (decimal * 100).toFixed(precision) + '%';
    }
    
    // 序数格式化 (1st, 2nd, 3rd, etc.)
    static toOrdinal(num) {
        const suffix = ['th', 'st', 'nd', 'rd'];
        const v = num % 100;
        return num + (suffix[(v - 20) % 10] || suffix[v] || suffix[0]);
    }
}

console.log(NumberFormatter.addCommas(1234567));    // "1,234,567"
console.log(NumberFormatter.formatBytes(1048576));  // "1 MB"
console.log(NumberFormatter.toPercent(0.1234));     // "12.3%"
console.log(NumberFormatter.toOrdinal(21));         // "21st"
```

## 8. 性能优化和最佳实践

### 8.1 性能对比

```javascript
// 数值转换性能对比
function performanceTest() {
    const iterations = 1000000;
    const testValue = "123.45";
    
    // 方法1: Number()
    console.time('Number()');
    for (let i = 0; i < iterations; i++) {
        Number(testValue);
    }
    console.timeEnd('Number()');
    
    // 方法2: parseFloat()
    console.time('parseFloat()');
    for (let i = 0; i < iterations; i++) {
        parseFloat(testValue);
    }
    console.timeEnd('parseFloat()');
    
    // 方法3: 一元加号
    console.time('Unary +');
    for (let i = 0; i < iterations; i++) {
        +testValue;
    }
    console.timeEnd('Unary +');
}

// performanceTest(); // 取消注释运行性能测试
```

### 8.2 最佳实践

#### 数值验证

```javascript
// ✅ 推荐的数值检查方式
function isNumber(value) {
    return typeof value === 'number' && !isNaN(value) && isFinite(value);
}

// ✅ 更严格的整数检查
function isInteger(value) {
    return Number.isInteger(value) && Number.isSafeInteger(value);
}

// ❌ 避免这样的检查
function badNumberCheck(value) {
    return !isNaN(value); // 会对非数字类型进行隐式转换
}
```

#### 浮点数处理

```javascript
// ✅ 正确的浮点数比较
function compareFloats(a, b, epsilon = 1e-10) {
    return Math.abs(a - b) < epsilon;
}

// ✅ 金额计算使用整数
function calculatePrice(basePrice, taxRate) {
    const baseCents = Math.round(basePrice * 100);
    const taxCents = Math.round(baseCents * taxRate);
    return (baseCents + taxCents) / 100;
}

// ❌ 直接进行浮点数运算
function badCalculatePrice(basePrice, taxRate) {
    return basePrice + (basePrice * taxRate); // 可能有精度问题
}
```

#### 类型转换

```javascript
// ✅ 明确的类型转换
function processUserInput(input) {
    if (typeof input === 'string') {
        const num = Number(input);
        if (isNumber(num)) {
            return num;
        }
    }
    throw new Error('Invalid number input');
}

// ❌ 隐式转换
function badProcessInput(input) {
    return +input; // 可能产生意外结果
}
```

## 9. 常见陷阱和错误

### 9.1 类型混淆

```javascript
// ❌ 常见错误：混淆字符串和数字
console.log("5" + 3);        // "53" (字符串拼接)
console.log("5" - 3);        // 2   (数字运算)
console.log("5" * "3");      // 15  (都转为数字)

// ✅ 明确的处理方式
function safeAdd(a, b) {
    const numA = Number(a);
    const numB = Number(b);
    if (isNumber(numA) && isNumber(numB)) {
        return numA + numB;
    }
    throw new Error('Both arguments must be valid numbers');
}
```

### 9.2 精度问题

```javascript
// ❌ 直接比较浮点数
if (0.1 + 0.2 === 0.3) {
    console.log("相等"); // 不会执行
}

// ✅ 使用精度容忍
if (Math.abs((0.1 + 0.2) - 0.3) < Number.EPSILON) {
    console.log("相等"); // 会执行
}
```

### 9.3 NaN 处理

```javascript
// ❌ 错误的 NaN 检查
function badNaNCheck(value) {
    return value === NaN; // 永远返回 false
}

// ✅ 正确的 NaN 检查
function goodNaNCheck(value) {
    return Number.isNaN(value) || (value !== value);
}
```

## 10. ES6+ 新特性

### 10.1 BigInt 类型（处理大整数）

```javascript
// 当需要处理超出 Number.MAX_SAFE_INTEGER 的整数时
const bigNum1 = 123n;                    // BigInt 字面量
const bigNum2 = BigInt(123);             // BigInt 构造函数
const bigNum3 = BigInt("12345678901234567890");

console.log(typeof bigNum1);             // "bigint"
console.log(bigNum1 + bigNum2);          // 246n

// 注意：不能混合 Number 和 BigInt 运算
// console.log(bigNum1 + 123);           // TypeError
console.log(bigNum1 + BigInt(123));      // 246n

// 转换
console.log(Number(123n));               // 123 (可能丢失精度)
console.log(String(123n));               // "123"
```

### 10.2 数值分隔符（ES2021）

```javascript
// 提高大数字的可读性
const million = 1_000_000;
const binary = 0b1010_0001;
const hex = 0xFF_EC_DE_5E;
const decimal = 123.456_789;

console.log(million);    // 1000000
console.log(binary);     // 161
console.log(hex);        // 4293844574
console.log(decimal);    // 123.456789
```

## 11. 总结

### 核心要点

1. **类型特性**：
    
    - JavaScript 只有一种数字类型（双精度浮点数）
    - 特殊值：`NaN`、`Infinity`、`-Infinity`
    - 安全整数范围：`-(2^53-1)` 到 `2^53-1`
2. **类型转换**：
    
    - `Number()` 函数进行显式转换
    - `parseInt()`、`parseFloat()` 用于字符串解析
    - 一元加号 `+` 可快速转换
3. **精度问题**：
    
    - 浮点数计算可能有精度误差
    - 使用 `Number.EPSILON` 进行精确比较
    - 金额计算建议使用整数（分为单位）
4. **最佳实践**：
    
    - 使用 `Number.isNaN()` 而非 `isNaN()`
    - 使用 `Number.isFinite()` 而非 `isFinite()`
    - 明确类型转换，避免隐式转换
    - 处理大整数时考虑使用 `BigInt`
5. **常见陷阱**：
    
    - `NaN === NaN` 返回 `false`
    - `typeof NaN` 返回 `"number"`
    - 字符串与数字的 `+` 运算是拼接，不是相加
    - 空字符串和空数组转数字时为 `0`


[[BigInt 类型|下一篇： BigInt]]