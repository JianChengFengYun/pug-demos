安装依赖：
    npm install

将src中pug模板 编译成HTML到dist：
    gulp

# pug(jade)模板-demos

参考链接：

https://zhuanlan.zhihu.com/p/25177324

https://pug.bootcss.com/language/attributes.html


**嵌套-注释 demo1**

1. Pug 中，缩进表示元素的嵌套，而空格之后的文本表示闭合元素 <?> 和 </?> 之间的部分。

2. Pug 当中，是不需要表示与 开标签 对应的 闭标签 的。

3. // 和 //- 的区别在于，前者会被转换成 HTML 的注释，而后者表示 Pug 文件的注释，但不会被生成 HTML 的一部分。

```pug 
    div
        p hello guys
    p Mr Li is our leader

    //- this line will be ignored.

    // this line will be genetated.
```

**id-class-其他属性 demo2**

1. 如果一个标签中既含有子元素，又含有文本，而文本又在子元素之后怎么办呢？即：

```html
    <div>
        <p> Hello world.</p>
        Nice to meet you!
    </div>
```

类似于 Shell 编程中的管道概念，我们使用 ’|‘ 符号在独立的一行之前，表示把其后的文本“注入”缩进所确定的父元素当中，且相对位置按照行顺序排列
```pug
    div 
        p Ziyuan is handsome
        | no I can't agree
        p so you a stick 
```
2. CSS 选择器中我们用 ‘.’ 来表达 class 规则，而用 ‘#’ 来表达 id 规则
3. 除了 class 和 id 之外的其他属性
    - 括号()
    - 使用 &attributes 语法操作属性对象

**插值 demo3**

1. 使用连词符 - 读作 “dash” 作为一行的行首，然后使用 JavaScript 语法定义变量，然后使用 #{ } 插值语法来在其中使用字面表达式语句来填充。值得注意的是，#{} 插值是一种转义语法，其中所有的特殊字符都会被转义，比如：
```pug
    - var code = "<code></code>";
    div #{code}
```

2. 另一种插值叫非转义插值，使用 !{} 插值语法，比如：
```pug
    - var code = "<code></code>";
    div !{code}
```

3. 第三种插值是指（HTML）标签插入，使用 #[] 语法，括号内使用 Pug 的语法进行，比如
```pug
    div
        | this is a pug #[code code sequence].
```
将会生成：

```html
    <div>this is a pug <code>code sequence</code>.</div>
```
正式由于这样的标签插入语法的出现，我们大大减少了行内元素（如：\<em>, \<strong> 等）的出现

使用 | 管道插入文本的几率，也使得我们的代码更加简洁。


**迭代 demo4**

Pug 当中，有两个迭代关键字，分别是 each/ for 和 while。

1. each 常常用在需要从一个数组中遍历元素，或是一个对象中遍历属性的情况之下

```pug
    - var array = ["Dage", "Ziyuan", "Ruiqi", "Yudan"];
    ul 
        each val, idx in array
            li #{val} is at !{idx}
```

each 后面的两个分别是迭代当中的当前元素（属性）和当前的索引顺序（从0开始）（其中索引顺序可以省略），而 in之后紧接的，就是需要遍历的数组或是对象。

2. while 语法，用于循环一定次数来迭代数组，使用方法：

```pug
    - var n = 0;
    - var ourGroup = ["Ling", "Yan", "Li", "Jun", "Hong", "Jie", "Yun"];
    ul
        while n < ourGroup.length
            li #{ourGroup[n]} now at #{n++} 
```

待更新...