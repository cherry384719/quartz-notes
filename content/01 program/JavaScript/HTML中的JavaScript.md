### 如何将 JavaScript 引入网页
1. **引用`<script>`元素**

     a.   直接在网页`<script>`元素中嵌入JavaScript代码

    ```html
    <script>
    function sayHello(){
	    console.log('Hello!');
	}
    </script>
    ```
     
     >但是要注意，此时被包含在其中的js代码中就不能出现或输出`</script>`，防止浏览器报错,如果真的要输出这个字符串，可以使用转义字符，改为：`<\/script>`
     
	​    b.   包含外部js文件
	```html
	<script src="example.js"></script>
	<!-->其中的example.js是外部js文件的路径<-->
	```

2. **将`<script>`元素放置在正确位置** 
	