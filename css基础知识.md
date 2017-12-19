## 块级元素和行内元素分别有哪些？测试并列出4条以上的特性区别？
**块级元素：**
```
div h1~h6 p hr form ul dl ol pre table li dd dt tr td th
```
**行内元素：**
```
em strong span a br img button input label select textarea code script 
```
**块级元素和行内元素的区别**
1. 块级元素占据一整行空间；行内元素占据的空间由自身内容宽度决定。
2. 块级元素可以包含块级元素和行内元素；行内元素只能包含行内元素和文本。
3. 块级元素可以设置宽度、高度；行内元素设置无效。
4. 块级元素设置padding，四个方向都有效；行内元素只有左右方向有效。
5. 块级元素设置margin，四个方向都有效；行内元素只有左右方向有效。

## 如何让块级元素水平居中？如何让行内元素水平居中?
- 块级元素：margin-left:auto;margin-right:auto;
- 行内元素:父元素上text-align: center;

## a:link, a:hover, a:active, a:visited 的顺序是怎样的？ 为什么？
顺序：a：link，a：visited，a：hover，a：active

这4个选择器是a标签的不同行为，优先级都一样。浏览器解析css样式是从上到下，后面的会覆盖前面的。所以为了保证特殊状态的样式能正常展示，就放在靠后的。

## 选择器的优先级是怎样的?对于复杂场景如何计算优先级？
**选择器的优先级从高到低：**

1. 在属性后面使用 !important 会覆盖页面内任何位置定义的元素样式
2. 作为style属性写在元素标签上的内联样式
3. id选择器
4. 类选择器
5. 伪类选择器
6. 属性选择器
7. 标签选择器
8. 通配符选择器
9. 浏览器自定义

**对于复杂场景计算优先级：**
把选择器划分为以下等级(等级由高到低)：
- 行内样式==>a
- ID选择器==>b
- 类,属性选择器和伪类选择器==>c
- 标签选择器、伪元素==>d

查看各个等级的个数，由高到低比较，同一级别的个数越多，优先级就越高，当同一级别的个数相等时，对比下一等级的个数，以此类推。

## IE 盒模型和W3C盒模型有什么区别？
IE 盒模型：box-sizing: border-box; W3C盒模型:box-sizing: content-box;(默认盒模型)

**区别：**
1. W3C盒模型：设置的元素width和height，只是内容的宽高度。
2. IE 盒模型: 设置的元素的width和height，实际上是内容的宽度+左右内边距+左右边框；内容的高度+上下内边距+上下边框

## `*{ box-sizing: border-box;}`的作用是什么？
匹配任何的元素，按照指定的IE盒模型来计算宽度和高度

## 让一个元素"看不见"有几种方式？有什么区别？
1. display:none; 元素不在占据文档流的空间
2. visibility:hidden; 元素不可见，但还占据空间
3. opacity: 0; 透明度为0，但还占据空间
4. height: 0; 块级元素设置高度为0
5. background-color: rgba(0,0,0,0.2) 设置元素的背景为透明，元素仍占据空间。

## inline-block有什么特性？如何去除缝隙？高度不一样的inline-block元素如何顶端对齐？
`inline-block`具有块级元素的特性可以设置宽度、高度、margin、padding；又有行内元素的特性不占据一正行空间，由自身内容宽度决定。

**去除缝隙：**
`inline-block`元素间留白间距出现的原因就是**标签段之间的空格**

1. 去除html标签之间的空白，缝隙就没了，但这样会导致代码连在一起，可读性差。
2. 在父元素上设置font-size:0;,在`inline-block`元素上重新设置font-size。

**高度不一样的inline-block元素如何顶端对齐**
都设置vertical-align: top;

## px, em, rem 有什么区别？
- px: 固定单位
- em：相对单位，相对父元素字体的大小
- rem: 相对单位，相对根元素(html)字体的大小

## line-height: 2和line-height: 200%有什么区别?
```
//html
<div class="parent">
    <div class="son">
      子元素<br/>
      子元素
    </div>
</div>
//css
 .parent {
      line-height: 2/200%;
      font-size: 16px;
  }
  .son {
      font-size: 30px;
  }
```
以上面的代码为例子：

- line-height: 2;父元素的行高设置为2时，会根据子元素的字体大小动态计算出行高值让子元素继承。所以，子元素行高等于30px * 2 = 60px;
- line-height: 200%；父元素的行高设置为200%时，会根据父元素的字体大小先计算出行高值然后再让子元素继承。所以，子元素的行高等于200%*16px = 32px;

## 单行文本溢出加 (...)如何实现？
```
white-space: nowrap;  //空白间隙不换行
overflow: hidden;     //溢出隐藏
text-overflow: ellipsis; //溢出的文本用...代替
```

## 伪类和伪元素的区别？
- 伪类用于当前已有元素处于某种状态，为其添加对应的样式，根据用户行为而动态变化。
```
//例如a链接的4种状态
a:link、a:vistied、a:hover、a:active
```
- 伪元素是某个元素的子元素，逻辑上存在，但实际上不存在文档树中。


