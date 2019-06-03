## JS相关

### 1. 数组转成字符串的方式

1. 使用toString方法 转成字符串

例如 var arr = [1,2,3]

arr.toString() ==  '1,2,3';

2. 使用join()方法 转成字符串

例如 var arr = [1,2,3]

arr.join(',') ==  '1,2,3';
还可以自由控制需要什么隔开例如 - , 如果不需要什么隔开就写空
arr.join('') ==  '123';

### 2. JS数组的排序方式

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

### 3. JSON转换

1. 将JS对象转成JSON字符串
var str = JSON.stringify(obj);

2. 将JSON字符串转成JS对象

var obj = JSON.parse(str); 


### 4. 对原型链的理解

1. 什么是原型链
    简单来说就是对象和构造函数之间连接的一个链条 简称原型链
2. 原型链的作用
    可以通过原型链继承构造函数对象定义的一些公共的属性方法
3. 原型链的流程图

    ![原型链流程图](./原型链流程图.png)