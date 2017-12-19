## BFC(Block Formatting Contexts)块级格式化上下文
块级格式化上下文是页面上一个独立渲染的区域，容器里的子元素在布局上不会影响到外面的元素。
浮动元素和绝对定位元素（absolute，fixed），非块级盒子的块级容器（例如 inline-block, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的块级格式化上下文。
