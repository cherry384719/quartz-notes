# JavaScript 闭包详解笔记

## 什么是闭包？

闭包（Closure）是JavaScript中一个重要且强大的概念。简单来说，**闭包是指一个函数能够访问并操作其外部作用域中的变量，即使在其外部函数已经执行完毕之后**。

### 核心特点

- 函数嵌套：内部函数引用外部函数的变量
- 变量持久化：外部函数执行完毕后，被引用的变量仍然存在
- 作用域链：内部函数可以访问外部函数的作用域

## 基础示例

### 示例1：最简单的闭包

```javascript
function outerFunction(x) {
    // 这是外部函数的变量
    let outerVariable = x;
    
    // 内部函数
    function innerFunction(y) {
        // 内部函数访问外部函数的变量
        return outerVariable + y;
    }
    
    return innerFunction;
}

const closure = outerFunction(10);
console.log(closure(5)); // 输出: 15
```

在这个例子中，`innerFunction` 形成了一个闭包，它可以访问 `outerVariable`，即使 `outerFunction` 已经执行完毕。

### 示例2：计数器闭包

```javascript
function createCounter() {
    let count = 0;
    
    return function() {
        count++;
        return count;
    };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1()); // 1
console.log(counter1()); // 2
console.log(counter2()); // 1 (独立的计数器)
console.log(counter1()); // 3
```

每个计数器都维护着自己独立的 `count` 变量，这就是闭包的威力。

## 闭包的实际应用

### 1. 数据私有化

```javascript
function createPerson(name) {
    let privateName = name;
    
    return {
        getName: function() {
            return privateName;
        },
        setName: function(newName) {
            if (newName && newName.length > 0) {
                privateName = newName;
            }
        }
    };
}

const person = createPerson("张三");
console.log(person.getName()); // "张三"
person.setName("李四");
console.log(person.getName()); // "李四"
console.log(person.privateName); // undefined，无法直接访问
```

### 2. 模块模式

```javascript
const myModule = (function() {
    let privateVariable = 0;
    
    function privateFunction() {
        console.log("这是私有函数");
    }
    
    return {
        publicMethod: function() {
            privateVariable++;
            privateFunction();
            return privateVariable;
        },
        getPrivateVariable: function() {
            return privateVariable;
        }
    };
})();

console.log(myModule.publicMethod()); // "这是私有函数", 1
console.log(myModule.getPrivateVariable()); // 1
```

### 3. 函数工厂

```javascript
function createMultiplier(multiplier) {
    return function(x) {
        return x * multiplier;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

### 4. 事件处理中的闭包

```javascript
function attachListeners() {
    for (let i = 0; i < 3; i++) {
        setTimeout(function() {
            console.log("按钮 " + i + " 被点击");
        }, 100);
    }
}

attachListeners(); 
// 输出：
// 按钮 0 被点击
// 按钮 1 被点击  
// 按钮 2 被点击

// 如果使用 var 而不是 let，会出现问题：
function attachListenersWrong() {
    for (var i = 0; i < 3; i++) {
        setTimeout(function() {
            console.log("按钮 " + i + " 被点击"); // 都会输出 3
        }, 100);
    }
}
```

## 常见陷阱和注意事项

### 1. 循环中的闭包问题

```javascript
// 问题代码
var funcs = [];
for (var i = 0; i < 3; i++) {
    funcs[i] = function() {
        return i; // 所有函数都会返回 3
    };
}

// 解决方案1：使用 let
var funcs2 = [];
for (let i = 0; i < 3; i++) {
    funcs2[i] = function() {
        return i;
    };
}

// 解决方案2：立即执行函数
var funcs3 = [];
for (var i = 0; i < 3; i++) {
    funcs3[i] = (function(index) {
        return function() {
            return index;
        };
    })(i);
}
```

### 2. 内存泄漏风险

```javascript
function createBigClosure() {
    let bigData = new Array(1000000).fill('data');
    
    return function smallFunction() {
        // 只使用一个小变量，但整个 bigData 都被保持在内存中
        return "hello";
    };
}

// 解决方案：及时清理不需要的引用
function createOptimizedClosure() {
    let bigData = new Array(1000000).fill('data');
    let smallData = bigData[0]; // 只保留需要的数据
    bigData = null; // 清理大数据
    
    return function() {
        return smallData;
    };
}
```

## 闭包的优缺点

### 优点

- **数据封装**：提供私有变量的能力
- **状态保持**：函数可以记住状态
- **模块化**：创建独立的功能模块
- **回调函数**：在异步编程中保持上下文

### 缺点

- **内存占用**：变量不会被垃圾回收
- **性能影响**：访问闭包变量比局部变量慢
- **调试困难**：增加了作用域链的复杂性

## 实践练习

### 练习1：银行账户模拟

```javascript
function createBankAccount(initialBalance) {
    let balance = initialBalance;
    
    return {
        deposit: function(amount) {
            if (amount > 0) {
                balance += amount;
                return balance;
            }
            return "无效金额";
        },
        withdraw: function(amount) {
            if (amount > 0 && amount <= balance) {
                balance -= amount;
                return balance;
            }
            return "余额不足或无效金额";
        },
        getBalance: function() {
            return balance;
        }
    };
}

const account = createBankAccount(1000);
console.log(account.deposit(500)); // 1500
console.log(account.withdraw(200)); // 1300
console.log(account.getBalance()); // 1300
```

### 练习2：缓存函数

```javascript
function memoize(fn) {
    let cache = {};
    
    return function(...args) {
        let key = JSON.stringify(args);
        
        if (cache[key]) {
            console.log("从缓存获取结果");
            return cache[key];
        }
        
        console.log("计算新结果");
        let result = fn.apply(this, args);
        cache[key] = result;
        return result;
    };
}

// 使用示例
const slowFunction = function(num) {
    // 模拟耗时操作
    for (let i = 0; i < 1000000000; i++) {}
    return num * 2;
};

const fastFunction = memoize(slowFunction);
console.log(fastFunction(5)); // 计算新结果, 10
console.log(fastFunction(5)); // 从缓存获取结果, 10
```

## 总结

闭包是JavaScript中的核心概念，它让函数拥有了"记忆"的能力。理解闭包对于掌握JavaScript的高级特性、模块化编程和函数式编程都至关重要。在实际开发中，闭包广泛应用于数据封装、模块设计、事件处理等场景。

掌握闭包需要：

1. 理解作用域链的工作原理
2. 明白变量的生命周期
3. 学会识别和创建闭包
4. 了解闭包的应用场景和潜在问题
5. 在实践中不断加深理解

---

## 思维导图

```
JavaScript 闭包
├── 定义与概念
│   ├── 函数嵌套
│   ├── 访问外部作用域
│   └── 变量持久化
├── 工作原理
│   ├── 作用域链
│   ├── 词法环境
│   └── 变量对象
├── 基础示例
│   ├── 简单闭包
│   ├── 计数器
│   └── 返回函数
├── 实际应用
│   ├── 数据私有化
│   │   ├── 私有变量
│   │   └── 访问控制
│   ├── 模块模式
│   │   ├── IIFE
│   │   └── 命名空间
│   ├── 函数工厂
│   │   ├── 参数固化
│   │   └── 定制函数
│   └── 事件处理
│       ├── 状态保持
│       └── 上下文绑定
├── 常见问题
│   ├── 循环陷阱
│   │   ├── var 问题
│   │   └── 解决方案
│   ├── 内存泄漏
│   │   ├── 引用保持
│   │   └── 及时清理
│   └── 性能影响
│       ├── 内存占用
│       └── 访问速度
├── 优缺点
│   ├── 优点
│   │   ├── 数据封装
│   │   ├── 状态保持
│   │   ├── 模块化
│   │   └── 回调支持
│   └── 缺点
│       ├── 内存消耗
│       ├── 性能开销
│       └── 调试复杂
└── 最佳实践
    ├── 合理使用
    ├── 内存管理
    ├── 性能优化
    └── 代码可读性
```