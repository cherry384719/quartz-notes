BigInt 是一个数字原始类型，可以表示并安全地操作任意精度的整数，突破了 Number 类型的整数范围限制。

- **用途**： 处理需要超出 `Number.MAX_SAFE_INTEGER` (2^53 - 1) 的大整数运算，例如高精度时间戳、大数据或加密算法中。
- 示例：
```js
// 在数字末尾加上 'n' 来创建 BigInt
const veryLargeNumber = 90071992547409919007199254740991n;

const result = veryLargeNumber * 2n;

console.log(result); // 输出: 180143985094819838014398509481982n

// 注意：BigInt 不能与 Number 类型直接进行混合运算
// console.log(veryLargeNumber + 10); // 这会抛出 TypeError
```

[[String 类型|下一篇： String 类型]]
