## CSS相关

### 3. css3中box-shadow存在的问题 

1. css3中box-shadow的溢出问题
    子容器加了box-shadow在父容器中可能存在溢出 可以通过给父元素添加overflow:hidden解决
2. box-shadow阴影被覆盖问题
    1. 使用相对定位解决position:relative和z-index提高层数可以把阴影显示出来
    2. 开启gpu加速, 设置3d效果 { transform: translate3d(0, 0, 0); } 

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