[TOC]

##  \==操作符 和  \=== 操作符的区别

​	==操作符  宽松类型比较   会进行类型转换 "1" == [1] //true

​	===操作符  严格类型比较   比较双方类型不同 即为false

​      Object.is(val1,val2) 不会强制类型转换但与  === 有些许区别

```js
Object.is(NaN,NaN)//true
NaN === NaN//false
Object.is(+0,-0)//false
+0 === -0//true
```



##  何为Getter Setter  ,它们和Vue等框架有什么关系?

所谓Getter 和 Setter  其实是对象的属性以函数来表示

Todo:为何这样做   之前无论是读取 对象的属性  还是 设置对象的属性   开发者无法从中介入

使用Getter Setter  可以实现  数据与 视图 的双向绑定

具体表现: 外界获取对象属性时   获得Getter的函数返回的值 ,   为属性赋值时  调用Setter修改



给出一个对象,将其所有的属性变为Getter 和 Setter

```js
function observe(obj){
 	var initial  = {}
    for(let key in obj){
        if(obj.hasOwnProperty(key)){
           initial[key] = obj[key]
            Object.defineProperty(obj,key,{
                
                get:function(){
                    return initial[key]
                },
                set: function(val){
                    initial[key] = val
                } 
            })
        }
    }
	return obj
}
//
//问题1   爆栈  解决办法
// 问题2   块级作用域  使用let 替代  var
//问题三   不能暴露原有属性 
// 问题4    对象是个深层次的递归对象
```

## JS  this的指向  列出所有的情况

### 为什么需要this

​		使用this  可以更加优雅的传递"隐式"传递对象引用 而不用显式传递上下文对象

> this在函数运行时产生,但是this既不指向函数自身或者函数的词法作用域.
>
> this指向完全取决于函数的调用方式 how the function is called

### 绑定规则

- 默认绑定

  `独立函数调用`  时   this 指向 全局变量

  1. 如何判断应该应用默认绑定

     分析`调用位置`

  2. 在严格模式下  全局环境无法绑定this  此时 this 绑定 undefined

- 隐式绑定

  调用位置是否有上下文对象

  1. 首先注意  foo() 的声明方式,及其如何被 obj 对象  引用

     无论是在 obj 中  定义  还是  被obj的属性引用   严格来讲  函数都不属于对象

  2. **但是**  调用位置会使用obj上下文来引用函数,  因此 可以认为 在函数**调用** 的时候,被obj **"拥有"**   this 指向 obj

  > tip :   对象属性引用链中只有最顶层或者说是最后一层会影响调用位置  



  ```js
  var obj = {
  	a: 2,
  	foo: foo,
      bar: function(){
          console.log(this.a)
      } 
  }
  var obj2 = {
      a: 333,
      obj: obj
  }
  window.a = 222// 直接给a 赋值  在 window 上无法直接访问 window.a
  function foo(){
      console.log(this.a)
  }
  var b = obj.foo
  b()//222
  obj.foo()//2
  foo()//222
  obj.bar()//2
  obj2.obj.foo()// 2  不会去寻找obj2 的里的 a 即便obj中不存在a  this 还是指向obj  但其值为undefined
  ```

- 显式绑定

  使用[call](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call) , [apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) ,  [bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 等函数进行this的绑定

  第一个参数为   你想将  this 绑定到 的地方 但是需要注意的是，指定的`this`值并不一定是该函数执行时真正的`this`值

  > 如果这个函数处于[非严格模式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)下，则指定为 `null` 或 `undefined` 时会自动替换为指向全局对象，原始值会被包装

- new绑定




- 意外情况

  - 隐式绑定丢失绑定对象,导致this应用默认规则 绑定到window  或者 undefined(严格模式)

    - var b = obj.foo ; b() 赋值操作导致this丢失

    - 调用回调函数 会导致赋值操作   从而  this丢失

      ```js
      function foo() {
      	bar()
      }
      function bar(){
          console.log( this.a );
      }
      function doFoo(fn) {
      	// fn 其实引用的是foo
      	fn(); // <-- 调用位置！
      }
      var obj = {
      	a: 2,
      	foo: foo
      };
      var a = "oops, global"; // a 是全局对象的属性
      debugger;doFoo( obj.foo ); // "oops, global  
      
      // 触发赋值操作  fn = obj.foo  调用栈 doFoo => foo  其调用位置 在 doFoo  独立调用没有附带上下文  
      ```


```js
腾讯面试题
var x = 20;
var a = {
 	 x: 15,
	 fn: function() {
 		var x = 30;
 		return function() {
  			return this.x
 		}	
 	}
}
console.log(a.fn());//function
console.log((a.fn())());//20
console.log(a.fn()());//20
console.log(a.fn()() == (a.fn())());//true
console.log(a.fn().call(this));//20
console.log(a.fn().call(a));//15
```



## 类 Class

面试题:  用js实现一个进度条动画

```js
Class Proress_bar {
    constructor(){
        this.curr = 0
        this.outer = document.create
    },
    start(){
        
    },
    end(){
            
    },
    add(){
            
    },
    set(){
            
    }
}
```





## 闭包







## JSONP





## tcp的三次握手 和四次挥手