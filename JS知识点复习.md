###  何为Getter Setter  ,它们和Vue等框架有什么关系?

所谓Getter 和 Setter  其实是对象的属性以函数来表示

Todo:为何这样做   之前无论是读取 对象的属性  还是 设置对象的属性   开发者无法从中介入

使用Getter Setter  可以实现  数据与 视图 的双向绑定

具体表现: 外界获取对象属性时   获得Getter的函数返回的值 ,   为属性赋值时  调用Setter修改

```js
function observe(obj){
    var arr = Object.keys(obj);
    arr.forEach(it => {
        get it
    })
    
    for(var key in obj){
        if(obj.hasOwnProperty(key)){
            Object.defineProperty(obj,key,{
                get:
                
                
                set: 
            })
        }
    }
}
```



### JS  this的指向  列出所有的情况

```js

var obj = {
    bar : 3,
    foo : a,
    abc : ()=>{
       return this.bar
    },
    xyz : ()=>{
	  return a()        
    } 
} 
function a(){
   return this.bar
}
var bar = 333
console.log(obj.foo())//3
console.log(a())//333
console.log(obj.abc())
console.log(obj.abc.call(obj,1))
console.log(obj.xyz())
console.log(obj.xyz.call(obj))
```



```js
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
console.log(a.fn());//
console.log((a.fn())());//20
console.log(a.fn()());//30  20
console.log(a.fn()() == (a.fn())());//false true
console.log(a.fn().call(this));//20
console.log(a.fn().call(a));//15
```





### 闭包







### JSONP





### tcp的三次握手 和四次挥手