## CSS相关

### 3. css3中box-shadow存在的问题 

1. css3中box-shadow的溢出问题
    子容器加了box-shadow在父容器中可能存在溢出 可以通过给父元素添加overflow:hidden解决
2. box-shadow阴影被覆盖问题
    1. 使用相对定位解决position:relative和z-index提高层数可以把阴影显示出来
    2. 开启gpu加速, 设置3d效果 { transform: translate3d(0, 0, 0); } 