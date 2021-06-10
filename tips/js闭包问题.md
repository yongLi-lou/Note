# js闭包问题

闭包的形成

> 当内部函数被保存到外部时，会形成闭包；闭包会导致原始作用域链不释放，造成内存泄漏（占用）；

例如：



```html
<script type="text/javascript">
	function test(){
		var arr = [];
		for(var i = 0; i < 10; i++){
			arr[i] = function(){
			console.log(i);
			}
		}
		return arr;
	}
	var myArr = test();
	for(var j = 0;j<10;j++){
		myArr[j]();
	}
</script>
```

最后输出的结果是10个10；

使用let