# html
## 浏览器内核
主要分成两部分：渲染引擎(layout engineer或Rendering Engine)和JS引擎。渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。  

JS引擎则：解析和执行javascript来实现网页的动态效果。

最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎
## XMLHttpRequest
0:刚创建，未调用open()方法  
1:执行open()还未执行send()  
2:执行send()  
3:服务器开始响应  
**4:响应结束**
```
let xhr=new XMLHttpRequest()
xhr.onreadystatuschange=function(){
    if(xhr.readystate===4){
        if(xhr.status===200){
            alert(xhr.responseText)
        }
    }
}
xhr.open('get',url,false)
xhr.send()
```
## img 
title属性是鼠标滑到元素上时显示  
alt是img的特有属性，是图片内容的等价描述，用于图片无法加载时显示、读屏器阅读图片。可提图片高可访问性
## 全局属性
- contextmenu: 自定义鼠标右键弹出菜单内容
- class:为元素设置类标识，多个类名用空格分开，CSS和javascript可通过class属性获取元素
- data-*: 为元素增加自定义属性

## web语义化
通过区域使HTML文档更加清晰，文档结构化，便于浏览器和搜索引擎的解析  
 HTML标签的语义化是指：通过使用包含语义的标签（如h1-h6）恰当地表示文档结构  
 css命名的语义化是指：为html标签添加有意义的class
## 跨域
为了防止跨站请求伪造，浏览器会禁止非同源（协议+域名+端口 完全相同）的请求
- JSONP  
只能发送get请求
- http header CROS Access-Control-Allow-Origin
- 空iframe加form
- Nginx代理
- 前端在webpack-dev-server中通过proxy进行代理

## 从浏览器输入URL到显示页面的步骤
1. 查看浏览器是否有缓存，通过http请求头 expires和cache-control（max-age）控制
2. 从URL中解析协议、主机、端口、path
3. tcp链接建立后发送http请求
4. 浏览器检查状态码
5. 检查是否需要缓存
6. 对响应解码（例如gzip压缩）
7. 解析HTML构建dom树，下载资源，执行js

window.open(url) 其中url最前面加上斜杠，使用绝对路径

## 从浏览器接收到URL到开启网络请求线程
浏览器是多进程的，每个tab页都会开启一个进程
### 状态码
1xx——指示信息，表示请求已接收，继续处理  
2xx——成功，表示请求已被成功接收、理解、接受  
3xx——重定向，要完成请求必须进行更进一步的操作  
4xx——客户端错误，请求有语法错误或请求无法实现  
5xx——服务器端错误，服务器未能实现合法的请求

## sessionStorage localStorage cookie
1. 有效期 cookie在浏览器关闭前有效 sessionStorage在窗口关闭前有效 localStorage长期有效
2. 共享：sessionStorage不能共享，localStorage在同源文档之间共享，cookie在同源且符合path规则的文档之间共享
3. cookie 可以设置maxAge,有效期

## cookie session token
[link](https://www.jianshu.com/p/bd1be47a16c1)

## 问题总结
react 服务端渲染 vue特性 区别 http let const var 组件懒加载 页面缓存 响应式布局 前端缓存  jQuery事件解绑 组件通信、传值 闭包
# css
## display
1.   display:none;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；visibility: hidden;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见
1. display: none;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；visibility: hidden;是继承属性，子孙节点消失由于继承了hidden，通过设置visibility: visible;可以让子孙节点显式
1. 修改常规流中元素的display通常会造成文档重排。修改visibility属性只会造成本元素的重绘。

# javascript
## null与undefined
- undefined 变量已经声明，但未赋值 typeof undefined==='undefined'
- null 一个空对象 typeof null ==="object"   

**执行上下文，运行环境（全局,函数）**
## 执行上下文
当控制器转到可执行代码时，就会进入一个执行上下文（当前代码的执行环境，形成一个作用域）  
JavaScript运行环境包括：
- 全局环境
- 函数环境
- eval环境

一个JavaScript中会有多个执行上下文，JavaScript引擎以栈的方式处理，栈底永远是全局上下文，栈顶是当前正在执行的上下文，栈顶的上下文执行完毕后会自动出栈
## 执行上下文的生命周期
![image](http://upload-images.jianshu.io/upload_images/599584-391af3aad043c028.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1. 创建阶段  
执行上下文会分别创建变量对象，建立作用域链，以及确定this的指向，此时会有变量提升，函数生命在最前面，接着是变量声明，var声明变量时遇到同名声明会跳过，而不会覆盖
2. 执行阶段

## 闭包
闭包和作用域是有关系的  
函数在定义的词法作用域以外执行，函数可以继续访问定义时的词法作用域
### 作用域
var 声明的变量只有全局和函数两个作用域  
没有使用var声明的变量具有全局作用域  
大括号包起来的叫做块级作用域 let const 使js具有了块级作用域
### 作用域链
由当前环境和上层环境的一系列变量对象组成，保证当前执行环境对符合访问权限的变量和函数的有序访问。  
### 闭包
**它由两部分组成。执行上下文(代号A)，以及在该执行上下文中创建的函数（代号B）。  
当B执行时，如果访问了A中变量对象中的值，那么闭包就会产生。**  
函数和声明该函数的词法环境的组合  
利用闭包突破作用域链，将函数内部和外部连接起来  
闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来  
闭包只能取得包含函数中任何变量的最后一个值  

### 闭包的作用
1. 封存变量
2. 延续局部变量的寿命
3. 回调函数实际就是在使用闭包

## this
this总是返回一个对象，简单说，就是返回属性或方法“当前”所在的对象,this是所有函数运行时的一个隐藏参数，指向函数的运行环境  
每个函数的this是在调用时绑定的，完全取决于函数的调用位置
### 绑定this的方法
**this的指向，是在函数被调用的时候确定的,取决于函数的调用方式**  

在一个函数上下文中，this由调用者提供，由调用函数的方式来决定。  
**如果调用者函数，被某一个对象所拥有，那么该函数在调用时，内部的this指向该对象。如果函数独立调用，那么该函数内部的this，则指向undefined。**  
但是在非严格模式中，当this指向undefined时，它会被自动指向全局对象。  

JavaScript提供了call、apply、bind这三个方法，来切换/固定this的指向  

函数实例的call方法，可以指定函数内部this的指向（即函数执行时所在的作用域）
```
var obj = {};

var f = function () {
  return this;
};

f() === this // true
f.call(obj) === obj // true
```
上面代码中，在全局环境运行函数f时，this指向全局环境；call方法可以改变this的指向，指定this指向对象obj，然后在对象obj的作用域中运行函数f  
call方法的参数，应该是一个对象。如果参数为空、null和undefined，则默认传入全局对象

apply方法的作用与call方法类似，也是改变this指向，然后再调用该函数。唯一的区别就是，它接收一个数组作为函数执行时的参数  
apply(..)是一个工具函数，适用于所有函数对象
```
func.apply(thisValue, [arg1, arg2, ...])
```

bind方法用于将函数体内的this绑定到某个对象，然后返回一个新函数  
分析函数的调用位置  
this是在运行时进行绑定的，它的上下文取决于函数调用时的各种条件  
### this隐式丢失
```
function foo(){
    console.log(this.a)
}
var obj={
    a:'1',
    foo:foo,
}
var a='2'
var bar=obj.foo;
bar()// 2
```
虽然bar是ojb.foo的一个引用，实际上，他引用的是foo函数本身  
函数参数传递是值传递，**当使用回调函数时，指向的是函数本身**  
箭头函数会继承外层函数调用的this绑定  
this丢失的情况
1. 匿名函数


## 构造函数和new命令：
JavaScript 语言的对象体系，不是基于“类”的，而是基于构造函数（constructor）和原型链（prototype）  
在严格模式中，函数内部的this不能指向全局对象，构造函数内部，this指的是一个新生成的空对象，所有针对this的操作，都会发生在这个空对象  
new的过程  
声明一个中间对象；  
将该中间对象的原型指向构造函数的原型；  
将构造函数的this，指向该中间对象；  
返回该中间对象，即返回实例对象。  
new命令总是返回一个对象，要么是实例对象，要么是return语句指定的对象
### 构造器属性
构造器属性是一个指向用于创建该对象的构造函数的引用
### 判断一个变量是否为数组

```
let arr=[];
1.
Object.prototype.toString.call(arr)==="[Object Array]"
2.
arr instanceof Object  // true
arr instanceof Array  // true
3.
Array.isArray(arr) // true
```


## 模块化
### CommonJS
每一文件就是一个模块，使用module.exports
### AMD规范
采用异步方式加载模块
AMD和CMD最大的区别是对依赖模块的执行时机处理不同，而不是加载的时机或者方式不同，二者皆为异步加载模块。

AMD依赖前置，js可以方便知道依赖模块是谁，立即加载；

而CMD依赖就近，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略。

CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。 

CommonJS 模块是运行时加载，ES6 模块是编译时输出接口

### 箭头函数
箭头函数表达式的语法比函数表达式更短，并且没有自己的this，arguments，super或 new.target。这些函数表达式更适用于那些本来需要匿名函数的地方，并且它们不能用作构造函数。
## 对象
类数组对象转为数组
```
function trigger() {
  var args = Array.prototype.slice.call(arguments);
  ...
}
```
## 浅复制与深复制
#### 浅复制  
1. 简单的引用复制
```
function shallowCopy(obj){
    let newObj={}
    for(let attr in obj){
        newObj[attr]=obj[attr]
    }
    return newObj;
}
```
2. Object.assign() 

#### 深复制  
1. Array的slice和concat方法不会改变原数组，会返回一个新数组，如果某个元素是对象，则拷贝的是引用，所以其实质还是浅拷贝
2. JSON parse stringify  
问题：对于正则表达式及函数会丢失响应的值，还有一点不好的地方是它会抛弃对象的constructor。也就是深拷贝之后，不管这个对象原来的构造函数是什么，在深拷贝之后都会变成Object。同时如果对象中存在循环引用的情况也无法正确处理
3. 
```
function deepCopy(obj){
 let newObj = obj instantof Array?[]：{}
    if(typeof obj!=='object'){
        return obj;
    }
    for(let att in ojb){
    if(typeof obj[att]==='object'){
        newobj[att]=deepCopy(obj[att])
    }esle{
        newObj[att]=obj[att]
    }
    }
    return newObj
}
```
### 对象的可枚举
enumerable 控制属性是否出现在对象的属性枚举中，例如 for...in 循环  
in 操作符会检查属性是否存在对象及其[[Prototype]]原型链中  
hasOwnProperty(..)只会检查是否存在对象中
### 面向对象
1. 构造函数
new关键字可以使一个函数变得与众不同  
步骤  
1. 创建一个中间对象  
1. 将中间对象的原型指向该构造函数  
1. 将构造函数的this指向中间对象  
1. 返回中间对象，即实例对象

所有函数都有prototype属性，指向一个对象，通过构造函数生成的对象的_proto_属性指向构造函数的prototype属性，该属性指向原型对象，原型对象的constructor方法指向构造函数
1.  原型   
构造函数解决了判断实例类型的问题，但还是一个对象的复制过程  
我们创建的每个函数都有一个prototype属性，该属性指向原型对象
![image](http://upload-images.jianshu.io/upload_images/599584-2fc7dad23d112791.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
构造函数是原型的一个属性
```
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.getName = function() {
        console.log('this is constructor.');
    }
}

Person.prototype.getName = function() {
    return this.name;
}

var p1 = new Person('tim', 10);

p1.getName(); // this is constructor.

function Person() {}

Person.prototype = {
    constructor: Person,
    getName: function() {},
    getAge: function() {},
    sayHello: function() {}
}
```
1.  原型链
基于原型链的继承
1. 继承
结合构造函数和原型来创建对象  
-构造函数继承（通过call/apply）
```
var Person = function(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.getName = function() {
    return this.name;
}

Person.prototype.getAge = function() {
    return this.age;
}

var Student = function(name, age, grade) {
    // 通过call方法还原Person构造函数中的所有处理逻辑
    Student.call(Person, name, age);
    this.grade = grade;
}


// 等价于
var Student = function(name, age, grade) {
    this.name = name;
    this.age = age;
    this.grade = grade;
}
```
- 原型继承  
子对象的原型成为父对象的一个实例
```
// 原型继承
Student.prototype = Object.create(Person.prototype, {
    // 不要忘了重新指定构造函数
    constructor: {
        value: Student
    }
    getGrade: {
        value: function() {
            return this.grade
        }
    }
})
```
- 构造函数和原型混合继承

```
function Parent(){
 		this.name = 'wang';
 	}

 	function Child(){
 		this.age = 28;
 	}
 	Child.prototype = new Parent();//继承了Parent，通过原型

 	var demo = new Child();
 	alert(demo.age);
 	alert(demo.name);//得到被继承的属性
```

## js执行机制(事件循环机制)
- macro-task(宏任务)：包括整体代码script，setTimeout，setInterval
- micro-task(微任务)：Promise，process.nextTick  
任务队列
事件循环的顺序，决定js代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。

## JSON

## html
### 事件
事件传播分为捕捉标签 到达对象 冒泡  
事件委托：利用事件的冒泡，解决事件处理程序过多的问题


# 框架
## 单向和双向数据绑定
单向数据绑定：  
双向数据绑定：UI界面中界面显示View与Model双向绑定，两者的变化会相互影响，Vue通过数据劫持，通过Object.defineProperty()劫持各个属性的getter和setter

## react
### 性能优化
1. 纯组件（Pure Component）  
定义：如果一个组件只和 props 和 state 有关系，给定相同的 props 和 state 就会渲染出相同的结果，那么这个组件就叫做纯组件，换一句话说纯组件只依赖于组件的 props 和 state  
原理：重写shouldComponentUpdate,在该方法中对props及state进行浅比较（比较的效率尽量要大于重新渲染的效率）
```
/**
 * 浅比较两个对象是否相等
 * @param {Object} obj 
 * @param {Object} newObj 
 */
function shallowEqual(obj, newObj) {
    if (obj === newObj) {
        return true;
    }
    const objKeys = Object.keys(obj);
    const newObjKeys = Object.keys(obj);
    if (objKeys.length !== newObjKeys.length) {
        return false;
    }
    // 每个属性是否相等，不进行深入判断
    objKeys.forEach(key => {
        return obj[key] === newObj[key];
    })
}
```
官方提供PureRenderMixin插件

## redux
一个可预测的状态容器  
- 单一数据源 一个应用永远只有一个数据源
- 状态是只读的
- 状态修改均由纯函数完成

## react-redux
<Provider/> 和 connect（）实现Redux与React绑定
## Redux-middleware

