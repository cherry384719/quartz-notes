# 如何将 JavaScript 引入网页
## **1.引用`<script>`元素**

#### a.直接在网页`<script>`元素中嵌入JavaScript代码

```html
<script>
function sayHello(){
    console.log('Hello!');
}
</script>
```

 但是要注意，此时被包含在其中的js代码中就不能出现或输出`</script>`，防止浏览器报错,如果真的要输出这个字符串，可以使用转义字符，改为：`<\/script>`



#### 	b.包含外部js文件（==更推荐==，利于代码维护和页面缓存）

```html
<script src="example.js"></script>
<script src="https://www.example.com/main.js"></script>

<!-- 其中的example.js是外部js文件的路径，也可以是一个完整的URL -->
```
>按照惯例，外部 JavaScript 文件的扩展名是.js。这不是必需的，因为浏览器不会检 查所包含 JavaScript 文件的扩展名。这就为使用服务器端脚本语言动态生成 JavaScript 代 码，或者在浏览器中将 JavaScript 扩展语言（如 TypeScript，或 React 的 JSX）转译为 JavaScript 提供了可能性。不过要注意，服务器经常会根据文件扩展来确定响应的正确 MIME 类型。 如果不打算使用.js 扩展名，一定要确保服务器能返回正确的 MIME 类型。

**==注意==**：使用了`src`属性的`<script>`元素不应该在`<script>`和`</script>`标签中再包含其它JS代码。如果两者都写了，浏览器会**忽略** `<script>` 和 `</script>` 标签之间的任何内联 JavaScript 代码。



## **2.将`<script>`元素放置在正确位置** 

一般将`<script>`放在`<body>`元素中的页面内容之后，例如：

```html
<!DOCTYPE html>
<html>
	<head>
	<title>Document</title>
	</head>
	<body>
	<!-- 这里是页面内容 -->
	<script src="example1.js"></script>
	<script src="example2.js"></script>
	</body>
</html>
```

还有一些不常用的`<script>`标签，如`defer`、`async`、`<noscript>`，需要时查[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/HTML)
