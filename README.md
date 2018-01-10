安装依赖：
    npm install

将src中pug模板 编译成HTML到dist：
    gulp

### pug(jade)模板-demos

参考链接：

[Pug 中文文档](https://pug.bootcss.com/language/attributes.html)

http://jade-lang.com/reference

https://segmentfault.com/a/1190000006198621


**标签 - 文本(多行文本) - 注释 demo1**

1. 标签
    - Pug 中，缩进表示元素的嵌套

    ```pug
        a: img
    ```
    - 快展开

    ```pug
        doctype html
        html
            head
                title hello pug 
            body
                h1 pug pug
    ```

    - 自闭和标签

    Pug 还知道哪个元素是自闭合的：

    ```pug
        foo/
        foo(bar='baz')/
    ```

2. 标签空格之后的文本表示文本内容
    - 多行文本
    
    管道文本 :这是最简单的向模板添加纯文本的方法。只需要在每行前面加一个 | 字符（这个字符在类 Unix 系统下常用作“管道”功能，因此得名）。

    标签中的块: 有的时候您可能想要写一个非常大的纯文本块。一个典型的例子就是嵌入 Pug 的脚本或者样式。如果要这么做，只需要在标签后面接上一个 .（不要有空格）：

    ```pug

        div 
            p Ziyuan is handsome
            | no I can't agree
            p so you a stick 

        //- 多行文本
        p.
            asdfasdfa
            asdfasd
            adsfasd
            asdfasdfa
        script.
            if (usingPug)
                console.log('这是明智的选择。')
            else
                console.log('请用 Pug。')
        //- or
        p
            | dfas
            | dfas
            | dfas
    ```
    - 文本有标签 

    ```pug
        //- 文本有标签
        p
            | dfas <strong>hey</strong>
            | dfas
            | dfas
        //- or
        p
            | dfas 
            strong hey man
            | dfas
            | dfas
    ```

3. 单行注释和块注释

    - 单行注释

        // 和 //- 的区别在于，前者会被转换成 HTML 的注释，而后者表示 Pug 文件的注释，但不会被生成 HTML 的一部分。

        ```pug 

            //- this line will be ignored.

            // this line will be genetated.
        ```
    - 块注释

        ```pug
            //- 块注释 注意缩进
            div
                //
                    As much text as you want
                    can go here.
        ```
    - 条件注释

        Pug 没有特殊的语法来表示条件注释（conditional comments）。不过因为所有以 < 开头的行都会被当作纯文本，因此直接写一个 HTML 风格的条件注释也是没问题的。

        ```pug
            <!--[if IE 8]>
            <html lang="en" class="lt-ie9">
            <![endif]-->
            <!--[if gt IE 8]><!-->
            <html lang="en">
            <!--<![endif]-->
        ```


**id-class-其他属性 demo2**

1. CSS 选择器中我们用 ‘.’ 来表达 class 规则，而用 ‘#’ 来表达 id 规则

```pug
a.button
a(class="button")
```

2. 除了 class 和 id 之外的其他属性
    - 括号()

    可以用逗号作为属性分隔符，不过不加逗号也是允许的。
    ```pug
    a(href='baidu.com') 百度
    = '\n'
    a(class='button' href='baidu.com') 百度
    = '\n'
    a(class='button', href='baidu.com') 百度

    input(
        type='checkbox'
        name='agreement'
        checked
    )
    ```

    - 使用 &attributes 语法操作属性对象
    ```pug
        p#para&attributes({"A": "a", "B": "b"}) My paragraph.
    ```

3. 所有 JavaScript 表达式都能用：
```pug
- var authenticated = true
div(class=authenticated ? 'authed' : 'anon') aa
```

**嵌入 demo3**

用 - 开始一段不直接进行输出的代码

- 字符串嵌入 转义语法

    使用连词符 - 读作 “dash” 作为一行的行首，使用 JavaScript 语法定义变量，使用 #{ } 插值语法来在其中使用字面表达式语句来填充。值得注意的是，#{} 插值是一种转义语法，其中所有的特殊字符都会被转义，比如：

    ```pug
        - var code = "<code></code>";
        div #{code}
        p=code
    ```
- 字符串嵌入 非转义语法

    使用 !{} 插值语法，比如：

    ```pug
        - var code = "<code></code>";
        div !{code}
        p!=code
    ```
- 没有的变量赋值

    ```pug
    p=data;
    ```
    是空值而不是undefined  

- （HTML）标签插入 

    使用 #[] 语法，括号内使用 Pug 的语法进行，比如

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

1. each ：这是 Pug 的头等迭代方式，让您在模板中迭代数组和对象更为简便

```pug
    // each
    - var array = ["Dage", "Ziyuan", "Ruiqi", "Yudan"];
    ul 
        each val, idx in array
            li #{val} is at !{idx}

    // Pug 还让您能够迭代对象中的键值：
    ul
        each val, index in {'one':'一','two':'二','three':'三'}
            li= index + ': ' + val

    // 用于迭代的对象或数组仅仅是个 JavaScript 表达式，因此它可以是变量、函数调用的结果
    - var values = [];
    ul
    each val in values.length ? values : ['没有内容']
        li= val

    //还能添加一个 else 块,这个语句块将会在数组与对象没有可供迭代的值时被执行
    - var values = ['1'];
    ul
    each val in values
        li= val
    else
        li 没有内容

    // 也可以使用 for 作为 each 的别称。
    - var values = ['2'];
    ul
    for val in values
        li= val
    else
        li 没有内容

```

2. while 语法，用于循环一定次数来迭代数组，使用方法：

```pug
    - var n = 0;
    - var ourGroup = ["Ling", "Yan", "Li", "Jun", "Hong", "Jie", "Yun"];
    ul
        while n < ourGroup.length
            li #{ourGroup[n]} now at #{n++} 
```

**条件 demo5**

Pug 的条件判断的一般形式的括号是可选的，所以您可以省略掉开头的 -，效果是完全相同的。类似一个常规的 JavaScript 语法形式。

```pug
- var user = { description: 'foo bar baz' }
- var authorised = false
#user
  if user.description
    h2.green 描述
    p.description= user.description
  else if authorised
    h2.blue 描述
    p.description.
      用户没有添加描述。
      不写点什么吗……
  else
    h2.red 描述
    p.description 用户没有描述
```

Pug 同样也提供了它的反义版本 unless

```pug
//user.isAnonymous为false才会执行
unless user.isAnonymous
  p 您已经以 #{user.name} 的身份登录。

```


**Mixin 混入 demo6**

混入是一种允许您在 Pug 中重复使用一整个代码块的方法。可以传递一些参数

```pug
1.重复的代码块

mixin sayHi
    p hello everyone
+sayHi

编译后
<p>hello everyone</p>

2.传入参数

mixin pet(name)
  li.pet= name
ul
  +pet('cat')

3.blocks

mixin article(title)
  .article
      h1= title
      if block //是否有包含内容
        block
      else
        p No content provided
+article('Hello world')
+article('Hello world')
  p This is my

编译后：
<!--如果节点里面没有内容，就加上-->
<div class="article">
    <h1>Hello world</h1>
    <p>No content provided</p>
</div>
<div class="article">
    <h1>Hello world</h1>
    <p>This is my</p>
    <p>Amazing article</p>
</div>

4.Attributes

mixin link(href, name)
  //- attributes == {class: "btn"}
  a(class!=attributes.class, href=href)= name

+link('/foo', 'foo')(class="btn")
//- attributes已经转义，所以应该使用！=避免二次转义

编译后:
<a href="/foo" class="btn">foo</a>

5.不确定参数

mixin list(id, ...items)
  ul(id=id)
    each item in items
      li= item
+list('my-list', 1, 2, 3, 4)
//参数中要加入...，

编译后：
<ul id="my-list">
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
</ul>
```

**分支条件 Case demo7**

case 是 JavaScript 的 switch 指令的缩写，并且它接受如下的形式：

```pug
- var friends = 10
case friends
  when 0
    p 您没有朋友
  when 1
    p 您有一个朋友
  default
    p 您有 #{friends} 个朋友 

//- 分支传递 (Case Fall Through)
//- 在 JavaScript 中，传递会在明确地使用 break 语句之前一直进行。而在 Pug 中则是，传递会在遇到非空的语法块前一直进行下去。
- var friends = 0
case friends
  when 0
  when 1
    p 您的朋友很少
  default
    p 您有 #{friends} 个朋友

//- 如果您不想输出任何东西的话，您可以明确地加上一个原生的 break 语句：
- var friends = 0
case friends
  when 0
    - break
  when 1
    p 您的朋友很少
  default
    p 您有 #{friends} 个朋友

//- 块展开
- var friends = 1
case friends
  when 0: p 您没有朋友
  when 1: p 您有一个朋友
  default: p 您有 #{friends} 个朋友
```