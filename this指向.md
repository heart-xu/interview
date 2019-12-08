## question 1
```js
// 作为对象方法调用 和 全局函数调用时 this 的指向问题

var val =  1;

var obj =  {
  val:  2,
  dbl:  function()  {
    val *=  2;
    this.val *=  2;
    console.log('val:', val);
    console.log('this.val:',  this.val);
  }
};
// 说出下面的输出结果
obj.dbl();
var fn1 = obj.dbl;
fn1(); 
```
<font color="red">解析</font>
```js
/*
    obj.dbl()  是对象方法调用,此时dbl函数中的this指向的是obj
    val *2 ==> val = val *2 
    首先在当前函数作用域中查找val变量,没有找到,此时向外层作用域查找
    知道全局作用域找到 'var val = 1' , 得到val = 1 , 执行函数后,val = 2

    this.val ==>  显然此处的this指向的是obj中的this,所有this.val --> obj.val = 2
    即 this.val *2 = 4
*/
//执行完obj.dbl() 后 ,全局中的val变成了2 , obj中的val属性值变为4

/*
	var fn1 = obj.dbl;  将dbl的函数体用一个变量接收 , 此时fn1() 就是函数调用模式,this指向 window
	此时执行  val *= 2;   this.val *= 2;   这里的val 和 this.val都是全局的val
	所以全局的val 连续乘以 2 , 即 val *2 *2 = 8
	此时下面两个打印
		val = 8
		this.val = 8
*/
```

## question 2
```js
function f4(){
  var name = 'stone';
  var f5 = function(){
      console.log(name);
      console.log(this);
  }
  return f5;
}
f4()();
```
<font color="red">解析</font>
```js
//f4()调用返回f5函数,再进行调用用  所以这个调用方式为函数调用模式, this指向window
//函数f5 获取 函数f4的变量, 是经典闭包模式
```

## question 3
```js
   function a(n) {
       this.x = n;  
       return this; 
   }

   var x = a(5);  // x = window  x 是window下面的属性
   // var y = a(6);

   console.log(x.x); 
   console.log(y.x);
```
<font  color ="red">解析 </font>
```js
// undefined  6
/*
var x = a(5)    
	先执行a(5) , 这是函数方法调用模式, this指向window, 所以内部代码
		window.x = 5		return window
	通过上述代码可知window下有个x属性,值为5
	
	此时var 一个变量 x 接收a(5)的返回值, x = window , 此时这个变量x会覆盖之前window下的x
	所以, 此时x就是window, 而window下也有一个名为x的属性, 指向window , x.x === window
	
var y = a(6)
	同理,在 window下又创建了个x 属性, 该属性又会覆盖上面的x , 此时window.x = 6
	返回 y = window
	
	
此时两行代码都执行完毕, 开始执行下面的打印语句
console.log(x.x)  //   x.x ==> 6.x  显然6 下面并没有x属性, 所有undefined
console.log(y.x)  //   y.x ==> window.x === 6
*/
```

