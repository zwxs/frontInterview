##  移动端相关

### 1. 移动端1px边框问题的解决方案 (华润集团)

1. 使用小数写px值 通过媒体查询判断设备的像素比 DRP 根据不同像素比写不同的边框大小

```css
.border { border: 1px solid #ccc }
@media screen and (-webkit-min-device-pixel-ratio: 2) {
    .border { border: 0.5px solid #ccc }
}
@media screen and (-webkit-min-device-pixel-ratio: 3) {
    .border { border: 0.333333px solid #ccc }
}

```

2. 使用边框图片

这样的1张6X6的图片, 9宫格等分填充border-image, 这样元素的4个边框宽度都只有1px

```css
@media screen and (-webkit-min-device-pixel-ratio: 2){ 
    .border{ 
        border: 1px solid transparent;
        border-image: url(border.gif) 2 repeat;
    }
}

```

3. 使用CSS3 box-shadow
```css
.shadow {
    -webkit-box-shadow:0 1px 1px -1px rgba(255, 0, 0, 0.5);
    box-shadow:0 1px 1px -1px rgba(255, 0, 0, 0.5);
}
```

4. viewport结合rem (推荐使用)

```html
//devicePixelRatio=2设置meta
<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">

//devicePixelRatio=3设置meta
<meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no">
```

5. 使用 :before     ,   :after 与  transform（推荐使用）

```css
//第一种方法
//构建1个伪元素, 将它的长宽放大到2倍, 边框宽度设置为1px, 再以transform缩放到50%
.radius-border{
    position: relative;
}
@media screen and (-webkit-min-device-pixel-ratio: 2){
    .radius-border:before{
        content: "";
        pointer-events: none; /* 防止点击触发 */
        box-sizing: border-box;
        position: absolute;
        width: 200%;
        height: 200%;
        left: 0;
        top: 0;
        border-radius: 8px;
        border:1px solid #999;
        -webkit-transform(scale(0.5));
        -webkit-transform-origin: 0 0;
        transform(scale(0.5));
        transform-origin: 0 0;
    }
}

@media screen and (-webkit-min-device-pixel-ratio: 3) {
    .radius-border:before{
        content: "";
        pointer-events: none; /* 防止点击触发 */
        box-sizing: border-box;
        position: absolute;
        width: 200%;
        height: 200%;
        left: 0;
        top: 0;
        border-radius: 8px;
        border:1px solid #999;
        -webkit-transform(scale(0.3333));
        -webkit-transform-origin: 0 0;
        transform(scale(0.3333));
        transform-origin: 0 0;
    }
}
```

### 2. 移动屏幕适配方案 (华润集团)

1. 使用rem+媒体查询实现
2. 使用rem+js实现
3. 使用rem+vw实现

rem是什么是一个相对单位参照根元素的字体大小 只要动态修改跟元素字体大小所有rem单位都会跟着改变
那么rem的根元素改变参照点是谁： 设计图 以设计图大小对应的的屏幕作为参照点  假设一个设计图大小对应的真实屏幕的根元素值 
例如750设计图真实屏幕是375 根元素100px 计算其他屏幕就使用 其他屏幕大小/375 * 100 求到其他屏幕的根元素大小

## CSS相关

### 3. css3中box-shadow存在的问题 (华润集团)

1. css3中box-shadow的溢出问题
    子容器加了box-shadow在父容器中可能存在溢出 可以通过给父元素添加overflow:hidden解决
2. box-shadow阴影被覆盖问题
    1. 使用相对定位解决position:relative和z-index提高层数可以把阴影显示出来
    2. 开启gpu加速, 设置3d效果 { transform: translate3d(0, 0, 0); } 

## JS相关

### 1. 数组转成字符串的方式 (智理软件)

1. 使用toString方法 转成字符串

例如 var arr = [1,2,3]

arr.toString() ==  '1,2,3';

2. 使用join()方法 转成字符串

例如 var arr = [1,2,3]

arr.join(',') ==  '1,2,3';
还可以自由控制需要什么隔开例如 - , 如果不需要什么隔开就写空
arr.join('') ==  '123';

### 2. JS数组的排序方式(智理软件)

1. 使用数组的sort函数可以实现排序

但是默认sort是按照ASCII编码排序的
如果需要按照数值的从小到大 从大到小需要使用第二个参数 是一个回调函数 回调函数需要两个参数 用来说明你要按照什么方式排序

例如a,b  
如果 a - b < 0 表示 a 比 b小 那么真实的a代表的数字就会排在b前面    
如果 a - b == 0 那么表示2个值相等 就按顺序排序
如果 a - b > 0 表示 a比b大 那么真实a代表的数组就会排在b后面

```js

function sortNumber(a, b) {
           
    return a - b  // 从小到大
    // return b - a  // 从大到小
}

var arr = [10, 5, 40, 25, 1000, 1]

document.write(arr + "<br />")
document.write(arr.sort(sortNumber));// 1 5 10 25 40 1000 从小到大
```

2. 自己使用算法实现排序 例如 冒泡排序

```js
var arr = [10, 20, 1, 2];
var t;
for(var i=0;i<arr.length;i++){
    for(j=i+1;j<arr.length;j++){
        if(arr[i]>arr[j]){
            t=arr[i];
            arr[i]=arr[j];
            arr[j]=t;
        }
    }
}
console.log(arr);  //[1, 2, 10, 20]
```

3. 快速排序

```js
// 快速排序
var quickSort = function(arr) {
    if(arr.length < 1) {//如果数组就是一项，那么可以直接返回
        return arr;
    }
    var centerIndex = Math.floor(arr.length / 2);//获取数组中间的索引
    var centerValue = arr[centerIndex];//获取数组中间项
    var left = [], right = [];
    for(var i = 0; i < arr.lenght; i++){
        if(arr[i] < centerValue){
            left.push(arr[i]);
        }else{
            right.push(arr[i]);
        }
    }
    return quickSort(left).contanct([centerValue], quickSort(right));//递归调用
}
```

### 3. JSON转换(智理软件)

1. 将JS对象转成JSON字符串
var str = JSON.stringify(obj);

2. 将JSON字符串转成JS对象

var obj = JSON.parse(str); 


### 4. 对原型链的理解(华润集团)

1. 什么是原型链
    简单来说就是对象和构造函数之间连接的一个链条 简称原型链
2. 原型链的作用
    可以通过原型链继承构造函数对象定义的一些公共的属性方法
3. 原型链的流程图

    ![原型链流程图](./JS相关/原型链流程图.png)

### 5. JS变量提升和自执行函数的面试题

```js
(function(){
    console.log(a);// undefined 虽然此时在函数内打印有a 因为var变量声明会提前所以不会报错 但是赋值不会提前所以a没有值是 undefined
    var a = b = 1;        
})();        
console.log(b);//1 因为 var a = b = 1 其实b并没在里面var所以 b是全局变量并且赋值为了1所以是1
console.log(a);// 因为 var a = b = 1; 这个var是在自执行函数里面的自执行函数是一个函数作用域 所以var a是函数内的局部变量所以外界拿不到 即是报错  a is not defined
```

### 6. JS this指向的面试题

```js

var name = '王五';
var obj = {
    name: '张三',
    getName:function(){
        return this.name;
    },
    children:{
        name:'李四',
        getName:function(){
            return this.name;
        }
    }
}    
console.log(obj.getName());//张三
console.log(obj.children.getName());//李四
var getName = obj.children.getName;
console.log(getName());//王五
// 那么如果我想调用getName() 但是想打印张三怎么办 可以使用call函数改变调用当前函数的this指针
console.log(getName.call(obj));//张三
```

### 7. 使用JS的类实现一个栈的类 并实现栈里面的常用API

```js
// 定义一个栈的类 实现栈的push pop getMax getMin等函数 
// 并实现先进后出 后进先出 而且能够不断求出 当前栈里面的数据的最大值最小值
class Stack {
    constructor(){
        this.dataStack = [];
        this.maxStack = [];
        this.minStack = [];

    }
    // 入栈
    push(item){
        if(this.minStack.length <= 0){
            this.minStack.push(item)
        }
        else if(item <= this.getMin()){
            this.minStack.push(item)
        }

        if(this.maxStack.length <= 0){
            this.maxStack.push(item)
        }
        else if(item >= this.getMax()){
            this.maxStack.push(item)
        }
        this.dataStack.push(item);                
    }
    // 出栈
    pop(){
        if(this.dataStack.length <= 0){
            return 
        }
        let value = this.dataStack.pop();
        
        if(value == this.getMin()){
            this.minStack.pop()
        }
        if(value == this.getMax()){
            this.maxStack.pop();
        }
        return value;
    }
    // 求最小值
    getMin(){               
        if(this.minStack.length <=0){
            return 0;
        }
        return this.minStack[this.minStack.length-1];
    }
    // 求最大值
    getMax(){             
        if(this.maxStack.length <=0){
            return 0;
        }                
        return this.maxStack[this.maxStack.length-1];
    }
}    
// 创建栈的对象
var stack = new Stack();
// 调用入栈
stack.push(3);
stack.push(5);
stack.push(10);
stack.push(7);
stack.push(6);
// 调用求最大值 最小值的函数
console.log(stack.dataStack);       
console.log(stack.getMax());
console.log(stack.getMin());
// 出栈
stack.pop();
stack.pop();
// 出栈后的最大值最小值
console.log(stack.getMax());
console.log(stack.getMin());
console.log(stack.minStack);
console.log(stack.maxStack);
stack.pop();
console.log(stack.getMax());
console.log(stack.getMin());
console.log(stack.minStack);
console.log(stack.maxStack);
stack.pop();
console.log(stack.getMax());
console.log(stack.getMin());
console.log(stack.dataStack);
console.log(stack.maxStack);
stack.pop();
console.log(stack.getMax());
console.log(stack.getMin());
console.log(stack.dataStack);
console.log(stack.maxStack);
```

## 存储相关


###  cookies sessionStorage和localstorage区别(华润集团)

相同点：都存储在客户端
不同点：1.存储大小

· cookie数据大小不能超过4k。

· sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

2.有效时间

· localStorage    存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；

· sessionStorage  数据在当前浏览器窗口关闭后自动删除。

· cookie          设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭

3. 数据与服务器之间的交互方式

· cookie的数据会自动的传递到服务器，服务器端也可以写cookie到客户端

· sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。


## vue相关 

### 1. react和vue有哪些不同，说说你对这两个框架的看法 (华润集团)

相同点

· 都支持服务器端渲染

· 都有Virtual DOM,组件化开发,通过props参数进行父子组件数据的传递,都实现webComponent规范

· 数据驱动视图

· 都有支持native的方案,React的React native,Vue的weex

不同点

· React严格上只针对MVC的view层,Vue则是MVVM模式

· virtual DOM不一样,vue会跟踪每一个组件的依赖关系,不需要重新渲染整个组件树.而对于React而言,每当应用的状态被改变时,全部组件都会重新渲染,所以react中会需要shouldComponentUpdate这个生命周期函数方法来进行控制

· 组件写法不一样, React推荐的做法是 JSX + inline style, 也就是把HTML和CSS全都写进JavaScript了,即'all in js'; Vue推荐的做法是webpack+vue-loader的单文件组件格式,即html,css,js写在同一个文件但是会区分开标签;

· 数据绑定: vue实现了数据的双向绑定,react数据流动是单向的

· state对象在react应用中不可变的,需要使用setState方法更新状态;在vue中,state对象不是必须的,数据由data属性在vue对象中管理

### 2. vue的双向绑定原理 (华润集团)


1. Vue.js 采用的是 数据劫持+发布/订阅模式 的方式，通过 Object.defineProperty() 来劫持各个属性的 setter/getter， 在数据变动时发布消息给订阅者（Wacther）, 触发相应的监听回调

2. 简单实现
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>双向绑定最最最初级demo</title>
        <meta charset="UTF-8">
    </head>
    <body>
        <div id="app">
            <input type="text" id="txt">
            <p id="show-txt"></p>
        </div>
    </body>
    <script>
        var obj={}
        Object.defineProperty(obj,'txt',{
            get:function(){
                return obj
            },
            set:function(newValue){
                document.getElementById('txt').value = newValue
                document.getElementById('show-txt').innerHTML = newValue
            }
        })
        document.addEventListener('keyup',function(e){
            obj.txt = e.target.value
        })
    </script>
</html>
```

### 2. vue的生命周期 （华润集团 炫酷网络）

1. 什么是vue的声明周期
Vue 实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。    
2. 组件第一次加载会触发哪些声明周期函数
第一次页面加载时会触发 beforeCreate, created, beforeMount, mounted 这几个钩子
3. DOM 渲染在 哪个周期中就已经完成
DOM 渲染在 mounted 中就已经完成了。
4. 声明周期各个函数的作用
生命周期钩子的一些使用方法： beforecreate : 可以在这加个loading事件，在加载实例时触发 created : 初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用 mounted : 挂载元素，获取到DOM节点 updated : 如果对数据统一处理，在这里写上相应函数 beforeDestroy : 可以做一个确认停止事件的确认框 nextTick : 更新数据后立即操作dom
5. 当keep组件激活时会调用哪个生命周期的函数
activated 该钩子在服务器端渲染期间不被调用。
6. 当keep-alive组件停用时调用。 
deactivated 该钩子在服务端渲染期间不被调用。 
7. keep的调用时机 （炫酷网络）

activated调用时机：

第一次进入缓存路由/组件，在mounted后面，beforeRouteEnter守卫传给 next 的回调函数之前调用：

    beforeMount=> 如果你是从别的路由/组件进来(组件销毁destroyed/或离开缓存deactivated)=>
    mounted=> activated 进入缓存组件 => 执行 beforeRouteEnter回调

    因为组件被缓存了，再次进入缓存路由/组件时，不会触发这些钩子：

    // beforeCreate created beforeMount mounted 都不会触发。
    所以之后的调用时机是：

    组件销毁destroyed/或离开缓存deactivated => activated 进入当前缓存组件 
    => 执行 beforeRouteEnter回调
    // 组件缓存或销毁，嵌套组件的销毁和缓存也在这里触发

8. 组件缓存和不缓存调用的生命周期

（1）不缓存： 
进入的时候可以用created和mounted钩子，离开的时候用beforeDestory和destroyed钩子,beforeDestory可以访问this，destroyed不可以访问this。

（2）缓存了组件： 
缓存了组件之后，再次进入组件不会触发beforeCreate、created 、beforeMount、 mounted，如果你想每次进入组件都做一些事情的话，你可以放在activated进入缓存组件的钩子中。 
同理：离开缓存组件的时候，beforeDestroy和destroyed并不会触发，可以使用deactivated离开缓存组件的钩子来代替

9. 完整的触发顺序

        触发钩子的完整顺序：

        将路由导航、keep-alive、和组件生命周期钩子结合起来的，触发顺序，假设是从a组件离开，第一次进入b组件：

        1. beforeRouteLeave:路由组件的组件离开路由前钩子，可取消路由离开。

        2. beforeEach: 路由全局前置守卫，可用于登录验证、全局路由loading等。

        3. beforeEnter: 路由独享守卫

        4. beforeRouteEnter: 路由组件的组件进入路由前钩子。

        5. beforeResolve:路由全局解析守卫

        6. afterEach:路由全局后置钩子

        7. beforeCreate:组件生命周期，不能访问this。

        8. created:组件生命周期，可以访问this，不能访问dom。

        9. beforeMount:组件生命周期

        10. deactivated: 离开缓存组件a，或者触发a的beforeDestroy和destroyed组件销毁钩子。

        11. mounted:访问/操作dom。

        12. activated:进入缓存组件，进入a的嵌套子组件(如果有的话)。

        13. 执行beforeRouteEnter回调函数next。

### 3. vue的computed属性 (华润集团)

1. computed用来监控自己定义的变量，该变量不在data里面声明，直接在computed里面定义，然后就可以在页面上进行双向数据绑定展示出结果或者用作其他处理
2. computed比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化，举例：购物车里面的商品列表和总金额之间的关系，只要商品列表里面的商品数量发生变化，或减少或增多或删除商品，总金额都应该发生变化。这里的这个总金额使用computed属性来进行计算是最好的选择
3. computed和watch的区别
watch主要用于监控vue实例的变化，它监控的变量当然必须在data里面声明才可以，它可以监控一个变量，也可以是一个对象
```js
　data(){
　　　return {
　　　　　　　　　　example0:"",
　　　　　　　　　　example1:"",
　　　　　　　　　　example2:{
　　　　　　　　　　　　inner0:1, 　　　　　　　　　
                　　　innner1:2 　　　　　　　　　
            　}
　　　　　　}
　　　　},
watch:{
　example0(newVal,oldVal){//监控单个变量
           ……
   }，
example2(newVal,oldVal){//监控对象
           ……
   }，
}
```
watch一般用于监控路由、input输入框的值特殊处理等等，它比较适合的场景是一个数据影响多个数据

计算属性可用于快速计算视图（View）中显示的属性。这些计算将被缓存，并且只在需要时更新

### 4. vuex (所有公司)

1. 什么时候需要使用vuex : 当项目业务复杂 页面之间很多关联数据需要交互的时候
2. 本地存储和vuex的区别

    1. vuex是状态管理，是为了解决跨组件之间数据共享问题的，一个组件的数据变化会映射到使用这个数据的其他组件当中。如果刷新页面，之前存储的vuex数据全部都会被初始化掉
    2. localStorage是H5提供的一个更简单的数据存储方式，之前是用cookie存放数据，但是cookie的数据量太小，所以就用localStorage，它可以有5M的限制，不受刷新页面的控制，长久保存

    3. 所以，在用vue进行项目开发的时候，什么时候用到vuex呢？

    当应用遇到多个组件共享状态时候，即：多个视图依赖于同一个状态，不同视图的行为需要变更同一状态。

    vuex的官网也说了，对于页面之间的传参对于多层嵌套组件将会很繁琐，而且对于兄弟组件之间的状态传递无能为力。所以就将这些组件的共享状态抽取出来，以一个全局单例模式管理，即vuex

        1.【响应式】vuex的状态存储是响应式的，当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会得到高效更新。

        2.【不能直接改变store】不能直接改变store的变化，改变store中状态的唯一途径是commit mutation。方便于跟踪每一个状态的变化

    4. 但是localstorage改变了不会自动动态更新视图需要手动获取更新

### 5. vuex中常见的属性 (所有公司)

1. vuex有哪几种属性
有五种，分别是 State、 Getter、Mutation 、Action、 Module
2. vuex的State特性是？
    1. Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于与一般Vue对象里面的data
    2. state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
    3. 它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中
3. vuex的Getter特性是？
    1.  getters 可以对State进行计算操作，它就是Store的计算属性
    2.  虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
    3.  如果一个状态只在一个组件内使用，是可以不用getters
4. vuex的Mutation特性是？
    1. Action 类似于 mutation，不同在于：
    2. Action 提交的是 mutation，而不是直接变更状态。
    3. Action 可以包含任意异步操作
5. Vue.js中ajax请求代码应该写在组件的methods中还是vuex的actions中？

    1. 如果请求来的数据是不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入vuex 的state里。
    2. 如果被其他地方复用，这个很大几率上是需要的，如果需要，请将请求放入action里，方便复用，并包装成promise返回，在调用处用async await处理返回的数据。如果不要复用这个请求，那么直接写在vue文件里很方便。

## ES6相关 (所有公司)

### 1.es6熟悉吗，说几个es6的新增的一些东西


1. 新增声明命令let和const
2. 模板字符串（Template String）
3. 函数的扩展(默认参数和箭头函数)
4. 对象扩展
5. import和export
6. Promise
7. 解构赋值
8. 展开运算符(...运算符)

### 2. let count的详解

1. let 和 const 都是块级作用域。以{}代码块作为作用域范围 只能在代码块里面使用。
2. 不存在变量提升，只能先声明再使用，否则会报错。在代码块内，在声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。
3. 在同一个代码块内，不允许重复声明。
4. const声明的是一个只读常量，在声明时就需要赋值。（如果 const 的是一个对象，对象所包含的值是可以被修改的。抽象一点儿说，就是对象所指向的地址不能改变，而变量成员是可以修改的。）

### 3. 模板字符串详解

用一对反引号(`)标识，它可以当作普通字符串使用，也可以用来定义多行字符串，也可以在字符串中嵌入变量，js表达式或函数，变量、js表达式或函数需要写在${ }中

```js
var str = `abc
def
gh`;
console.log(str);
let name = "小明";
function a() {
    return "ming";
}
console.log(`我的名字叫做${name}，年龄${17+2}岁，性别${'男'}，游戏ID：${a()}`);
```

### 4. 函数的扩展详解

1. 可以给函数参数设置默认值

```js
function A(a,b=1){
    console.log(a+b);
}
A(1);    //2
A(2,3);  //5
```

2. 箭头函数

    1. 可以省略function关键字 
    2. 可以不写函数名
    3. 如果只有一个表达式可以不需要写{} 和 返回值

```js

//省略写法
var people = name => 'hello' + name;
 
var getFullName = (firstName, lastName) => {
    var fullName = firstName + lastName;
    return fullName;
}

```

### 5. 对象的扩展详解

1. 属性的简写。ES6 允许在对象之中，直接写变量。这时，属性名为变量名, 属性值为变量的值
2. 方法简写

```js

var o = {
  method() {
    return "Hello!";
  }
};
 
// 等同于
var o = {
  method: function() {
    return "Hello!";
  }
};
```
3. Object.keys()方法，获取对象的所有属性名或方法名（不包括原形的内容），返回一个数组

```js

var obj={name: "john", age: "21", getName: function () { alert(this.name)}};
console.log(Object.keys(obj));    // ["name", "age", "getName"]
console.log(Object.keys(obj).length);    //3
 
console.log(Object.keys(["aa", "bb", "cc"]));    //["0", "1", "2"]
console.log(Object.keys("abcdef"));    //["0", "1", "2", "3", "4", "5"]

```

4. Object.assign ()，assign方法将多个原对象的属性和方法都合并到了目标对象上面。可以接收多个参数，第一个参数是目标对象，后面的都是源对象

```js

var target  = {}; //目标对象
var source1 = {name : 'ming', age: '19'}; //源对象1
var source2 = {sex : '女'}; //源对象2
var source3 = {sex : '男'}; //源对象3，和source2中的对象有同名属性sex
Object.assign(target,source1,source2,source3);
 
console.log(target);    //{name : 'ming', age: '19', sex: '男'}
```

### 6. import和export模块化引包和导出详解

1. export用于对外输出本模块（一个文件可以理解为一个模块）变量的接口。

2. import用于在一个模块中加载另一个含有export接口的模块。

3. import和export命令只能在模块的顶部，不能在代码块之中

```js
//导入部分
//全部导入
import Person from './example'
 
//将整个模块所有导出内容当做单一对象，用as起别名
import * as example from "./example.js"
console.log(example.name)
console.log(example.getName())
 
//导入部分
import { name } from './example'
 
//导出部分
// 导出默认
export default App
 
// 部分导出
export class User extend Component {};
```

### 7. Promise对象异步对象

1. Promise的概念和作用： Promise是异步编程的一种解决方案，将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数

2. Promise的状态： 它有三种状态，分别是pending进行中、resolved已完成、rejected已失败

3. Promise语法： Promise 构造函数包含一个参数和一个带有 resolve（解析）和 reject（拒绝）两个参数的回调。在回调中执行一些操作（例如异步），如果一切都正常，则调用 resolve，否则调用 reject。对于已经实例化过的 promise 对象可以调用 promise.then() 方法，传递 resolve 和 reject 方法作为回调。then()方法接收两个参数：onResolve和onReject，分别代表当前 promise 对象在成功或失败时

语法：
```js

var promise = new Promise((resolve, reject) => {
    var success = true;
    if (success) {
        resolve('成功');
    } else {
        reject('失败');
    }
}).then(
    (data) => { console.log(data)},
    (data) => { console.log(data)}
)
```

执行过程：

```js

setTimeout(function() {
    console.log(0);
}, 0);
var promise = new Promise((resolve, reject) => {
    console.log(1);
    setTimeout(function () {
        var success = true;
        if (success) {
            resolve('成功');
        } else {
            reject('失败');
        }
    },2000);
}).then(
    (data) => { console.log(data)},
    (data) => { console.log(data)}
);
console.log(promise);	//<pending> 进行中
setTimeout(function () {
    console.log(promise);	//<resolved> 已完成
},2500);
console.log(2);
 
//1
//Promise {<pending>}
//2
//0
//成功
//Promise {<resolved>: undefined}
```

### 8. 解构赋值详解

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）

1. 数组的解构赋值
    数组中的值会自动被解析到对应接收该值的变量中，数组的解构赋值要一一对应 如果有对应不上的就是undefined
    ```js
    
    var [name, pwd, sex]=["小周", "123456", "男"];
    console.log(name) //小周
    console.log(pwd)//123456
    console.log(sex)//男
    ```

2. 对象的解构赋值
    对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值
    ```js

    var obj={name:"小周", pwd:"123456", sex:"男"}
    var {name, pwd, sex}=obj;
    console.log(name) //小周
    console.log(pwd)//123456
    console.log(sex)//男
    
    //如果想要变量名和属性名不同，要写成这样
    let { foo: foz, bar: baz } = { foo: "aaa", bar: "bbb" };
    console.log(foz) // "aaa"
    console.log(foo) // error: foo is not defined
    ```


### 9. 展开运算符(...运算符)

1. 将字符串转成数组
```js
var str="abcd";
console.log([...str]) // ["a", "b", "c", "d"]
```
2. 将集合转成数组
```js
var sets=new Set([1,2,3,4,5])
console.log([...sets]) // [1, 2, 3, 4, 5]
```
3. 两个数组的合并
```js
var a1=[1,2,3];
var a2=[4,5,6];
console.log([...a1,...a2]); //[1, 2, 3, 4, 5, 6]
```
4. 在函数中，用来代替arguments参数
```js
rest参数  …变量名称

rest 参数是一个数组 ，它的后面不能再有参数，不然会报错

function func(...args){
    console.log(args);//[1, 2, 3, 4]
}
func(1, 2, 3, 4);
 
function f(x, ...y) {
    console.log(x);
    console.log(y);
}
f('a', 'b', 'c');     //a 和 ["b","c"]
f('a')                //a 和 []
f()                   //undefined 
```







## 2020-4-14更新

### 8. JS笔试题对象赋值属性考察

```js
    var fun = function(){

    }
    fun.prototype = {
        info:{
            name:'张三',
            age:20
        }
    }
    var a = new fun();
    var b = new fun();
    
    a.info.name = '李四';
    b.info.name = '王五';
    console.log(a.info.name) // 王五
    console.log(b.info.name)   // 王五 
    // 为何当前两个对象的info.name都输出王五 因为构造函数fun 并没有给new出来的每个实例对象赋予单独的info 对象这个属性 
    // 因为原型上的对象属性继承给实例后其实是把对象的内存地址和a.info b.info关联起来 其实a.info b.info都指的是同一个info对象
    // 那么a.info.name 和 b.info.name都找的是fun原型属性上的info 
    // 所以修改的是同一个值 取出来也是同一个值 故都是王五

    var fun = function () {

    }
    fun.prototype = {
        name: '张三',
        age: 20
    }
    var a = new fun();
    var b = new fun();
    a.name = '李四';
    b.name = '王五';
    console.log(a.name) // 李四
    console.log(b.name) // 王五
    // 为何此时打印的a 和 b的name又分别是 李四 和 王五 
    // 因为name是普通属性  构造函数创建实例后 原型上的普通属性会继承到实例对象上会形成2个不同的变量 所以a.name 和 b.name 一开始虽然值一样但是他们代表的变量不一样
    // 所以赋值改变不同后 a.name 和 b.name的属性是改变后的属性

    var fun = function () {
        this.info = {
            name: '张三',
            age: 20
        }
    }

    var a = new fun();
    var b = new fun();
    a.info.name = '李四';
    b.info.name = '王五';
    console.log(a.info.name)// 李四
    console.log(b.info.name)// 王五
    // 为何此时打印的又是李四 王五 因为 fun构造函数里面的 this当前实例对象 就是分别是 a 和 b 是两个不一样的实例对象 
    // 是分别给实例对象添加info对象属性 那么此时 a.info 和 b.info 是分别属于对象自己的 a.info.name  和 b.info.name也是不同的2个属性
    // 所以赋值取值后也是分别的

```

### 9. JS面试题 严格模式和非严格模式的考察

```js
    (function(){
        
        var a = b = 10;
    })();
    
    console.log(b) // 10
    //  为什么是b是10 呢 因为 上面的表达式虽然是写在一个只执行函数里面(闭包函数里面) 理论上 a和b在外面是无法访问到的
    // 但是由于表达式是 var a = b = 10; 其实只声明了a 并没有var声明b 所以a是函数内的局部变量 
    // 但是b由于没有被声明 在非严格模式下 其实是一个全局变量 所以外部也能访问到打印就是10
    (function(){
        'use strict'
        var a = b = 10;
        
    })();

    console.log(b) //  报错 b is not defined
    //  为什么是此时都是未定义呢 因为 上面的表达式虽然是写在一个只执行函数里面(闭包函数里面) 理论上 a和b在外面是无法访问到的
    // 但是由于表达式是 var a = b = 10; 其实只声明了a 并没有var声明b 所以a是函数内的局部变量 
    // 但是b由于没有被声明  但是当前模式是严格模式 严格模式下不允许使用未声明的变量 所以b是不存在的 无论在哪里打印都会报错不存在
```

### 10. 函数声明和函数表达式的区别

```js
    (function(){
        console.log(typeof foo) // function 为何此时foo打印的是function 因为 函数声明会提前虽然在后面定义的函数foo其实声明会在最前面
        console.log(typeof bar) // undefined  为何此时打印bar是undefined 因为bar是 函数表达式 相当于把函数通过表达式赋值给变量bar 表达式赋值不会提前所以此时还未定义就是undefined
        var foo = 'hello',
        bar = function(){
            return 'world';
        }
        function foo(){
            return 'hello'
        }
    })()
```

### 11. JS中什么是伪数组 有哪些常见的伪数组 如何转成普通数组

 伪数组: 就是拥有数组的可以存储多个值的特性并拥有长度length属性 但是不拥有普通数组的内置方法 例如push pop slice等方法
 哪些是常见的伪数组 DOM元素结构 jq获取的元素 这些都是伪数组

 伪数组转化成普通数组 需要用到数组原型中的方法 slice

 //需要使用到call方法，因为arr伪数组并没有该方法，需要使用call来借调 Array对象的slice方法
 console.log(Array.prototype.slice.call(arr));

 或者使用ES6的 console.log(Array.from(arr)) 函数也可以转化成普通数组

 ### 12. 函数返回值和 函数里面的this指向问题

 ```js
    var x = 20;
    var a = {
        x:15,
        fun:function(){
            var x = 30;
            return function(){
                return this.x;
            }

        }
    }
    console.log(a.fun()) //  ƒ (){return this.x;} a.fun的调用后的返回值是一个函数 所以输出就是当前返回的这个函数
    console.log((a.fun())()) // 20  a.fun() 调用后返回的函数再被表达式()包裹了起来 然后再调用 就是把当前返回的函数重新调用 但是当前返回的函数并没有赋值给谁那么调用者this指向window window的x是20
    console.log(a.fun()()) // 20 a.fun() 调用后返回的函数 然后再调用 就是把当前返回的函数重新调用 但是当前返回的函数并没有赋值给谁那么调用者this指向window window的x是20
    console.log(a.fun()() == (a.fun())()) // true 因为两种调用方法返回的结果都是20 所以为true
    console.log(a.fun().call(this)); // 20  将返回的函数使用call然后再传入this 此时的this还是window 所以函数里面的this也是window 所以还是外面的20
    console.log(a.fun().call(a)); // 15 将返回的函数call然后传入a 此时的函数里面的this指向的 当前对象a a有一个x为15 所以是a的15
 ```

 ### 13. 定义一个JS数组去重的方法

 ```js
        //1: 思路：定义一个新数组，并存放原数组的第一个元素，然后将元素组一一和新数组的元素对比，若不同则存放在新数组中。

        function unique(arr) {
            let newArr = [arr[0]];
            for (let i = 1; i < arr.length; i++) {
                let repeat = false;
                for (let j = 0; j < newArr.length; j++) {
                    if (arr[i] === newArr[j]) {
                        repeat = true;
                        break;
                    }else{
                        
                    }
                }
                if (!repeat) {
                    newArr.push(arr[i]);
                }
            }
            return newArr;
        }
 
        console.log(unique([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3, 5, 6, 7, 4]
        //2: 思路：先将原数组排序，在与相邻的进行比较，如果不同则存入新数组。

        function unique2(arr) {
            var formArr = arr.sort()
            var newArr=[formArr[0]]
            for (let i = 1; i < formArr.length; i++) {
                if (formArr[i]!==formArr[i-1]) {
                    newArr.push(formArr[i])
                }
            }
            return newArr
        }
        console.log(unique2([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3,  4,5, 6, 7]
         /3: 利用对象属性存在的特性，如果没有该属性则存入新数组。

        function unique3(arr) {
            var obj={}
            var newArr=[]
            for (let i = 0; i < arr.length; i++) {
                if (!obj[arr[i]]) {
                    obj[arr[i]] = 1
                    newArr.push(arr[i])
                }   
            }
            return newArr
        }
        console.log(unique2([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3, 5, 6, 7, 4]
        //4: 利用数组的indexOf下标属性来查询。

        function unique4(arr) {
            var newArr = []
            for (var i = 0; i < arr.length; i++) {
                if (newArr.indexOf(arr[i])===-1) {
                    newArr.push(arr[i])
                }
            }
            return newArr
        }
        console.log(unique4([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3, 5, 6, 7, 4]
        //5: 利用数组原型对象上的includes方法。

        function unique5(arr) {
            var newArr = []
            for (var i = 0; i < arr.length; i++) {
                if (!newArr.includes(arr[i])) {
                    newArr.push(arr[i])
                }
            }
            return newArr
        }
        console.log(unique5([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3, 5, 6, 7, 4]
        //6: 利用数组原型对象上的 filter 和 includes方法。

        function unique6(arr) {
            var newArr = []
            newArr = arr.filter(function (item) {
                return newArr.includes(item) ? '' : newArr.push(item)
            })
            return newArr
        }
        console.log(unique6([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3, 5, 6, 7, 4]
        //7: 利用数组原型对象上的 forEach 和 includes方法。

        function unique7(arr) {
            var newArr = []
            array.forEach(item => {
                return newArr.includes(item) ? '' : newArr.push(item)
            });
            return newArr
        }
        console.log(unique7([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3, 5, 6, 7, 4]
        //8: 利用数组原型对象上的 splice 方法。

        function unique8(arr) {
            var i,j,len = arr.length;
            for (i = 0; i < len; i++) {
                for (j = i + 1; j < len; j++) {
                    if (arr[i] == arr[j]) {
                        arr.splice(j, 1);
                        len--;
                        j--;
                    }
                }
            }
            return arr;
        }
        console.log(unique8([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        //9: 利用数组原型对象上的 lastIndexOf 方法。

        function unique9(arr) {
            var res = [];
            for (var i = 0; i < arr.length; i++) {
                res.lastIndexOf(arr[i]) !== -1 ? '' : res.push(arr[i]);
            }
            return res;
        }
        console.log(unique9([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));
        // 结果是[1, 2, 3, 5, 6, 7, 4]
        //10: 利用 ES6的set 方法。

        function unique10(arr) {
            //Set数据结构，它类似于数组，其成员的值都是唯一的
            return Array.from(new Set(arr)); // 利用Array.from将Set结构转换成数组
        }
        console.log(unique10([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]));

 ```

 ### 14. 定义一个长度为10 的数组并赋值为0-9

 ```js
    // 最简单最笨的方法 但是这样写的话 就显得的初级水平  如果定义长度为100 赋值为0-99就会很麻烦了 考点其实是数组的方法
    var arr = [0,1,2,3,4,5,6,7,8,9]

    // 一、使用Array.apply
    let arr= Array.apply(null, { length: 10 }).map((item,index)=>{
        return index;
        });
    console.log(arr);
    //(10) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

    //原理：Array.apply的第二个参数是类数组调用Array.apply(null, { length: 10 })等于生成了长度为10内容都为undefinded的数组

    // 二、使用Array.from
    let arr= Array.from({length:10}).map((item,index)=>{
        return index;
        });
    console.log(arr);
    //(10) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

    // 三、使用Array.prototype.fill
    let arr=  new Array(10).fill(1).map((item,index)=>{
        return index;
        });
    console.log(arr);
    //(10) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    
    //说明一下：new Array(10).fill(1)生成一个长度为10的空数组并用1填充
 ```

 ### 15. 封装一个JS函数满足以下要求

 ```js
        var num1 = 1;
        var num2 = 2;
        var num3 = 3;
        var num4 = 4;
        // computed 函数 计算函数  但是本身不计算 返回一个计算函数 再调用的时候在计算 把当前计算函数的参数和调用计算返回值的参数累加起来计算
        function computed(num1, num2) {
            return function (num) {                
                return num+num1+num2;
            }
        }
        // add 函数+函数 返回+当前数字
        function add(num) {
            return +num;
        }
        // mutiply函数 -函数 返回-当前数字
        function mutiply(num) {
            return -num;
        }
        console.log(computed(mutiply(num2), add(num3))(num1)); //  结果等于 num1-num2+num3 == 2

        var num1 = 1;
        var num2 = 2;
        var num3 = 3;
        var num4 = 4;

        function computed(num1, num2,num3) {
            return function (num) {       
               
                return num+num1+num2+num3;
            }
        }

        function add(num) {
            return +num;
        }

        function mutiply(num) {
            return -num;
        }
        // console.log(computed(mutiply(num2), add(num3))(num1)); //  num1-num2+num3 == 2
        console.log(computed(add(num2),mutiply(num3), add(num4))(num1)); //  num1+num2-num3+num4 == 4

        
        // 前面的方法如果有很多个数需要计算方法就要写很多次无法实现复用 下面是改造版本
        var num1 = 1;
        var num2 = 2;
        var num3 = 3;
        var num4 = 4;
        // ...args表示把所有单个项参数装在一个数组args里面
        function computed(...args) {
            return function (num) {       
                console.log(args)
                // 获取所有参数数组args 遍历  然后累加到num上 
                args.map((item) => {
                    num+=item
                })
                // 最后return num
                return num
            }
        }

        function add(num) {
            return +num;
        }

        function mutiply(num) {
            return -num;
        }
        console.log(computed(add(num2),mutiply(num3), add(num4))(num1)); //  num1+num2-num3+num4 == 4
 ```


### 16. 使用观察者模式实现一个事件监听器 包含 监听事件on 删除监听off 触发监听emit 满足以下执行结果

```js
 class EventEmitter {
    constructor() {
        this._events = {}
    }

    on(event, callback) { //监听event事件，触发时调用callback函数
        let callbacks = this._events[event] || []
        callbacks.push(callback)
        this._events[event] = callbacks
        return this
    }
    off(event, callback) { //停止监听event事件
        let callbacks = this._events[event]
        this._events[event] = callbacks && callbacks.filter(fn => fn !== callback)
        return this
    }
    emit(...args) { //触发事件，并把参数传给事件的处理函数
        const event = args[0]
        const params = [].slice.call(args, 1)
        const callbacks = this._events[event]
        callbacks.forEach(fn => fn.apply(this.params))
        return this
    }
    once(event, callback) { //为事件注册单次监听器
        let wrapFanc = (...args) => {
            callback.apply(this.args)
            this.off(event, wrapFanc)
        }
        this.on(event, wrapFanc)
        return this
    }

}

function query(val) {
    console.log("query");
}

function query2(val) {
    console.log("quer2");
}


var emitter = new EventEmitter();
console.log(emitter)
emitter.on("query", query);
emitter.on("query", query2);
emitter.emit("query"); // query query2
emitter.off("query", query);
emitter.emit("query"); // query2
```

### 4. 完成左中右布局 左边固定200px 中间自适应右边固定100px  中间部分优先渲染 并解释原理

```css
.box{
    padding-left: 200px;
    padding-right: 100px;
    height: 100px;
    position: relative;
    box-sizing:border-box;
}
.middle{
    height: 100px;
}
.left{
    width: 200px;
    height: 100px;
    position: absolute;
    left: 0;
    top: 0;
}
.right{
    width: 100px;
    height: 100px;
    position: absolute;
    right: 0;
    top: 0;
}
        
```

```html
<div class="box">
    <div class="middle"></div>  
    <div class="left"></div>
    <div class="right"></div>
</div>
```

1. 中间部分优先渲染其实就是指把中间的盒子放到最前面写 html的元素是从上往下执行 所以写在越前面越优先渲染
2. 布局 思路就是把左右2遍固定 宽高 分别定位到左右2边  然后父元素相对定位   并且给父元素设置左右的padding 把中间的content内容区域往里面挤 如果父元素的宽度要固定 最好加上一个box-sizing:border-box;防止padding 撑大盒子范围

### 5. 使用CSS画一个扇形 并说出实现原理

```css
.sector1 {
    width: 100px;
    height: 100px;
    border-radius: 100px 0 0;
    background: #ff0000; 
    /* 原理是利用 边框圆角的属性设置盒子的某一个角为圆角 然后其他角设置为0 然后给整个盒子加背景色就只显示出一个扇形角 */
}

.sector2 {
    width: 0;
    height: 0;
    border-radius: 100px;
    border-width: 100px;
    border-style: solid;
    border-color: red transparent transparent transparent;
    /* 原理是利用边框颜色可以分别设置的属性 设置其中一条边框有颜色其他边框没有颜色 在把整个盒子变为圆角盒子没有宽高只显示出边框部分  突出扇形 */
}

```