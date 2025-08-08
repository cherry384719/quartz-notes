null 类型只有一个值 null。它代表一个被开发者明确赋予的“空值”或“无对象”。

- **用途**：作为变量的初始值，表示该变量未来将指向一个对象，但现在还没有；或者用于清空一个对象变量。
- **示例**：
```js
let selectedUser = null; // 明确表示当前没有选中的用户
// ... 之后可能会被赋值为一个对象
// selectedUser = { name: '李四' };
```
- **注意**：typeof null 的结果是 "object"。这是一个在 JavaScript 最初版本中就存在的 bug，由于历史原因被保留了下来。

[[Boolean 类型|下一篇： Boolean 类型]]