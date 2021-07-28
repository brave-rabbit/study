# 三种样式

- 外部样式

```css
<link rel="stylesheet" href="./style.css">
```

- 内部样式

```css
<style>
        div{
            width: 200px;
            height: 200px;
            background-color: red;
        }
</style>
```

- 行内样式

```css
 <p style="width: 100px; height: 100px; background-color: blue;"></p>
```

优先级：	行间样式  > 内部样式和外部样式 （**就近原则**）



# css基本选择器

1.id选择器 #

- id值是唯一的
- 在css里 通过  #id名选中
- 不能以数字开头
- 区分大小写



2.class  选择器 .

- 一个标签上可以起多个class名
- 一个class是可以放在多个标签上的
- 在css里 通过  .class名 选中
- 不能以数字开头
- 区分大小写



3.标签选择器

- 通过标签名选中



4.通配符 *

- 选中的是页面中所有的标签
- 一般用来清除标签自带样式



5.优先级

id  >   class   >  标签选择器  >  * 



# 文字样式-可继承

```css
.p2{
            /* 字体大小 */
            font-size: 30px;
            /* 行高 */
            line-height: 60px;
            /* 字体 */
            font-family: '黑体1122232','楷体';
            /* 文字加粗 */
            font-weight: bold;
            /* 《=550  不加粗    》550加粗 */
            font-weight: 560;
            /* 文字倾斜 */
            font-style: italic;
            /* 综合简写 ： 字体大小 和  字体 是不能省略的，其他可省略不写*/
            font: italic bold 30px/60px '楷体';
    
            /* 文字颜色 */
            /* color: blue; */
            /* 首行缩进 : 单位 px  em(相对于当前字体大小的倍数)*/
            /* text-indent: 60px; */
            /* text-indent: 2em; */
            /* 文本水平对齐方式 : left（默认值） center  right  justify 两端对齐 */
            /* text-align: justify; */
    
    		/* 文字装饰  */
            /* text-decoration: underline; */下滑线
            /* text-decoration: overline; */上滑线
            /* text-decoration: line-through; */中滑线
            /* text-decoration: underline wavy red; 波浪线 */
            /* text-decoration: underline dashed red;  虚线*/
            /* text-decoration: underline dotted red; 点状线*/
            /* text-decoration: underline double red; 双实线 */
            /* text-decoration: underline solid red; */
        }
        
.p1{
 
            /* 小技巧：当行文字垂直居中，行高和盒子高度一致 */
            line-height: 100px;
}

.box{
            /* 子类标签可继承，但a标签除外 */
            color: green;
    		/* 缩进 */
            text-indent: 2em; 
            /* 给父元素设置后，里边的内联标签和内联块标签可在父元素里居中显示 */
            text-align: center;

        }
```



# 颜色和单位

颜色：

```css
        background-color: red;

		/* 六位十六进制数  取值 0-9 a-f */
        background-color: #526551;
     
        /* rgb   取值0-255 */
        background-color:rgb(152,123,107);
    
        /* RGBA A表示颜色的透明度 取值0-1  0完全透明  1完全不透明 */
        background-color: rgba(255,0,0,0.5);

        /* 透明 */
        background-color: transparent;
```
单位：

- px  具体像素
- em  相对于当前字体大小倍数
- %   相对于父元素的百分比



# 边框样式

```css
border-width: 10px;		
border-style: solid double dotted;
border-color: red blue green yellow;

border: 10px solid blue;
```

多个值：顺时针

- 一个值：四周
- 两个值： 上下   左右
- 三个值： 上  左右   下
- 四个值： 上   右   下    左

与此类似的还有 **padding 、margin**



## margin存在问题

- 重叠问题
  - 两个兄弟关系的盒子，margin值相遇会出现重叠，取其中最大值
  - 只给其中一个盒子设置marign即可
- 塌陷问题
  - 嵌套关系的盒子，父元素里的第一个子元素设置marign-top ， margin-top会传递给父元素
  - 解决办法
    - 避免使用margin-top
    - 给父元素设置 border-top 阻挡传递
    - 给父元素设置padding-top 阻挡传递



给块标签设置 marign:0px auto ;  可让改标签居中显示



## 内联标签设置内外边距

左右方向的内外边距可以设置，上下方向的内外边距无效



# 盒模型

盒模型组成： 实际内容宽高 +  padding + border + margin



![1625191241355](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1625191241355.png)



# 背景相关样式

```css
			/* 背景颜色 */
            background-color: red;
            /* 背景图片 */
            background-image: url(./1.png);
            /* 背景图是否平铺 */
            background-repeat: repeat;
            background-repeat:repeat-x;
            background-repeat:repeat-y;
            background-repeat: no-repeat;

            /* 背景图定位 一个值表示x轴位置，y轴默认居中   两个值：x轴 y轴 */
            background-position: 30px;
            background-position: 30px 50px;
            background-position: right bottom;
            background-position: center;
            background-position: 50% 50%;
			
			/*复合写法，可只设置颜色或只设置背景图片，其他不用可省略，多背景图，中间用逗号隔开*/
			background: red url(./1.png) no-repeat 30px 50px;
```



# 进阶选择器

```css
		/* 后代选择器 , 选中box里所有的p标签，不管嵌套多少层*/
        .box p{
            background-color: red;
        }

        /* 子代选择器 */
        .box>p{
            background-color: red;
        }

        /* 群组选择器 , 同时选中多个标签*/
        .box h3 , .p1 , h2{
            background-color: pink;
        }

        /* 相邻兄弟选择器 选中的是h3标签下边紧挨着是h2的标签 */
        h3 + h2{
            background-color:red;
        }

        /* 交集选择器，选中的是class名是p2的h4标签 */
        h4.p2{
            background-color: pink;
        }
```

## 进阶选择器优先级

- 基本选择器的权重  id  100    , class  10  ,  标签   1  ， *  0
- 权重越大优先级越高，权重是加法计算
- 权重一样，后边的覆盖前边的样式
- 遵循继承来样式优先级没有自己样式优先级高
- ！important 优先级最高，但是还是得遵循继承来样式优先级没有自己样式优先级高



## 伪类选择器

- 四个状态同时写需要注意先后顺序   link -->  visited  --> hover --> active

```css
        /* 未访问 */
        a:link{
            color: green;
        }

        /* 访问过后 */
        a:visited{
            color: red;
        }

        /* 鼠标移入 */
       a:hover{
            color: pink;
        }

        /* 鼠标按下 */
        a:active{
            color: yellow;
        }
    
      	/* 除a标签以外的其他标签，只有鼠标移入和鼠标按下 */
```



# 标签类型相互转换

- 转块标签  display : block;
- 转内联标签  display:inline;
- 转内联块标签 display:inline-block
- 隐藏标签  display:none
  - 隐藏后该标签不占位
  - 显示标签，只要display的值为  block  |  inline  |  inline-block即可显示



# 内联块垂直对齐方式

```
			vertical-align: baseline;  基线对齐（英文本第三行）
            vertical-align: top;
            vertical-align: middle;
            vertical-align: bottom;
            vertical-align: 0px;
```

- 块标签水平方向居中  margin：0px  auto;
- 内联标签和内联块标签水平对齐方式：  给父元素设置 text-align
- 文字垂直居中  line-height:盒子高度；
- 文字水平对齐方式  text-align



# 溢出操作

**超出部分不占位**

```css
			overflow: visible;  默认值，超出部分正常显示
            overflow: hidden;   超出部分隐藏
            overflow: scroll;   超出用滚动条显示
            overflow: auto;     超出有滚动条，不超出没有滚动条
```



# 伪元素

**必须和content样式配合使用**(相当于占位符)

```css
    .box::before{
        content: '离';
    }

    .box:after{
        content: '草';
    }
```


# 浮动

- float:left |  right

- 作用：让多个盒子在一行显示

- 特点

  - 标签设置浮动后不占位
  - 一行放不下会自动换行
  - 内联标签设置浮动后可设置宽高
  - 块标签设置浮动后，宽度默认由内容撑开


浮动问题：给子元素设置浮动后，父元素高度撑不起来

- 给父元素设置固定高

- 给父元素设置overflow:hidden|scroll|auto

- 在浮动元素的后边写空的块标签，给该标签设置 clear: both;

- 用伪元素代替第三种办法的空的块标签

  ```css
    .box:after{
              content:'';
              display: block;
              clear: both;
          }
  ```


# 图片间隙问题

```css
 img{
    /* 解决图片上下间隙问题 */
    /* vertical-align: top; */

     /* 解决一圈空隙 */
    /* float: left; */
}

.box{
    /* 解决一圈空隙 */
    font-size: 0px;
    line-height: 0px;
}
```



# 边框圆角

border-radius

```css
 			/* 四个角 */
            border-radius: 30%;

            /* 左上右下  右上左下 */
            border-radius: 20px  30px ;

            /* 左上   右上左下   右下 */
            border-radius: 20px  30px  40px;

            /* 左上  右上   右下   左下 */
            border-radius: 20px  30px  40px 50px;
```



# 定位

### 相对定位

position:relative;

- 特点
  - 只给标签设置position:relative;  对该标签没有任何影响
  - 设置相对定位后，该标签原来位置还占位
  - 配合top   left  right  bottom  使用
  - 参照物是当前标签原来的位置



### 绝对定位

position:absolute

- 特点
  - 参照物默认是页面第一屏，一般指定参照物（给父元素设置定位，即可形参参照物）**父相子绝**
  - 设置绝对定位以后，该标签不占位
    - 配合top left right bottom使用
  - 如果有多个父元素形成参照物，优先以离的最近的父元素作为参照物



### 固定定位

position:fixed;

- 特点
  - 参照物是浏览器屏幕
  - 设置固定定位后，该标签不占位
  - 配合top left right bottom使用



### z-index

- 标签默认层级为0
- 可通过z-index改变层级，值越大层级越高，值越小层级越低
- 必须配合定位使用



# 旋转缩放

### 旋转

transform:rotate(3deg)

- 单位： deg(度） 
- 正值顺时针旋转，负值逆时针旋转

```
 transform: rotate(30deg);
```



### 缩放

transform:scale(1)

- 值为0时，缩放到没有，>1放大   <1缩小
- 为负数时倒过来缩放
- 一个值，x轴y轴同时缩放
- 两个：x轴   y轴

```
			transform: scale(2);
            transform: scale(1,2);
            transform: scaleX(2);
            transform: scaley(2);
```



# 关键帧动画

```css
 		/* 1.定义动画 */
        @keyframes move{
            0%{
                width: 100px;
                height: 100px;
            }
            100%{
                width: 400px;
                height: 400px;
            }
        }
        
      
   		/* 2.应用动画 */
    	/* animation: move(动画名，必) 2s(运行时间，必) 4s(延迟时间) infinite(无限循环) alternate(有反向运动) linear(匀速); */

      	/* forwards 动画运行完成后，保持在100%的状态 */
      	animation: dong 4s forwards;
```































​	