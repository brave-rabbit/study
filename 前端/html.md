# HTML

## 1.标签分类

- 块标签：默认宽度100%  ， 独占一行，可设置所有样式
- 行内标签（内联标签）：默认宽度由内容撑开，多个内联标签在一行显示，部分样式设置无效（width 、 height)
- 行内块标签（内联块标签）：默认宽度由内容撑开，多个内联块标签在一行显示，可设置所有样式



## 2.常用元素的标签分类

### 2.1 块标签

默认宽度100%  ， 独占一行，可设置所有样式

- div
- p
- h1-h6
- hr：分割线
- ul：无序列表
- ol：有序列表 
- dl：自定义列表

```css
list-style: none;  //取消默认的列表前边装饰
```

表格相关的块标签：

- table

  - border 边框线
  - **cellspacing  单元格与单元格空隙**
  - **cellpadding  单元格里内容到边框线的空隙**
  - align   整个表格在浏览器中居中

  ```css
    /* border-collapse: separate;  默认值，边框线独立*/
       border-collapse: collapse;  /* 边框线合并*/
  ```

- tr：行

  - align  整个一行单元格内容居中

- caption：标题

- thead  表头

- tbody  表格主体内容

- tfoot  表格页脚

写thead tbody tfoot 好处：

1. 打乱顺序写，还可以保持从头到脚显示
2. 表格内容巨大时，可通过tbody将表格分段，加载一个tbody显示一个tbody

- form





### 2.2 行内标签

默认宽度由内容撑开，多个内联标签在一行显示，部分样式设置无效（width 、 height)

- span
- strong：加粗，更有强调意义
- b：加粗
- em：倾斜，更有强调意义
- i：倾斜
- sup：上标字体
- sub：下标字体
- del：删除标签
- br：换行标签
- a ：超链接
  - href：跳转路径（网址、相对路径、锚点连接（使用id））、下载文件（文件路径）
  - ta'rget：指定页面打开方式，**_self**(默认，当前窗口打开)   **_blank** 新窗口打开



### 2.3 行内块标签

默认宽度由内容撑开，多个内联块标签在一行显示，可设置所有样式

- img
  - src：图片路径
  - alt：图片加载不出来显示的内容
  - title：鼠标移入图片显示的内容

- th 单元格，小标题

- td 单元格

  - rowspan  跨行合并
  - colspan   跨列合并
  - align   当前单元格内容居中

- input 

  - type
    - ext  文本框
    - password 密码框
    - radio   单选按钮
    - checkbox  复选按钮
    - submit  提交
    - image 以图片形式提交
    - button  普通按钮
    - reset  重置按钮
  - name 表单提交时，以 name 去对应值
  - value 属性  默认值
  - maxlength  可输入的最大长度
  - minlength  可输入的最小长度
  - required   必填项 ， 不能为空
  - checked  单选按钮和复选按钮的默认选中项
  - readonly  只读，**可提交的**
  - disabled  禁用，**不可提交和修改**
  - placeholder  提示用户要输入什么内容

  ```css
  /* 取消输入框聚焦时默认的加粗边框线 */
   outline: none;
  ```

- selection 下拉列表

  - selected   下拉列表的默认选中项

- textarea  文本域

```css
     /* 禁止用户拖拽文本域 */
     resize: none;
```

- label  标签













