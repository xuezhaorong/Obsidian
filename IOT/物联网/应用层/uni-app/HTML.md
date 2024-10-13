## 元素
![image.png|650](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/13/21-58-38-e5761c565c00f70061df36c446589171-20241013215837-f41b20.png)

这个元素的主要部分有：

- **开始标签**（Opening tag）：包含元素的名称（本例为 _p_），被左、右角括号所包围。开头标签标志着元素开始或开始生效的地方。在这个示例中，它在段落文本的开始之前。
- **内容**（Content）：元素的内容，本例中就是段落的文本。
- **结束标签**（Closing tag）：与开始标签相似，只是其在元素名之前包含了一个斜杠。这标志着该元素的结束。没有包含关闭标签是一个常见的初学者错误，它可能会产生奇特的结果。

整个元素即指开始标签、内容、结束标签三部分组成的整体。

### 嵌套元素
可以把元素放到其他元素之中——这被称作**嵌套**。
```HTML
<p>My cat is <strong>very</strong> grumpy.</p>
```

### 块级元素和内联元素
- 块级元素在页面中以块的形式展现。一个块级元素出现在它前面的内容之后的新行上。任何跟在块级元素后面的内容也会出现在新的行上。块级元素通常是页面上的结构元素。例如，一个块级元素可能代表标题、段落、列表、导航菜单或页脚。一个块级元素不会嵌套在一个内联元素里面，但它可能嵌套在另一个块级元素里面。
- 内联元素通常出现在块级元素中并环绕文档内容的一小部分，而不是一整个段落或者一组内容。内联元素不会导致文本换行。它通常与文本一起使用，例如，`<a> 元素创建一个超链接，`<em\>` 和 `<strong\>` 等元素创建强调。

```html
<em>第一</em><em>第二</em><em>第三</em>

<p>第四</p>
<p>第五</p>
<p>第六</p>

```

`<em>` 是一个内联元素，所以就像你在下方可以看到的，第一行代码中的三个元素都没有间隙的展示在了同一行。而 `<p>`是一个块级元素，所以第二行代码中的每个 _p_ 元素分别都另起了新的一行展现，并且每个段落间都有一些间隔。

### 空元素 
不是所有元素都拥有开始标签、内容和结束标签。一些元素只有一个标签，通常用来在此元素所在位置插入/嵌入一些东西。这些元素被称为**空元素**。例如：元素 `<img>` 是用来在页面插入一张指定的图片。

```html
<img
  src="https://roy-tian.github.io/learning-area/extras/getting-started-web/beginner-html-site/images/firefox-icon.png"
  alt="Firefox 图标" />

```

## 属性
元素也可以拥有属性，属性看起来像这样：
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/13/22-17-08-4dbc83ff095fb72ea4b7d4de6fe8ca7c-20241013221708-74560b.png)
属性包含元素的额外信息，这些信息不会出现在实际的内容中。在上述例子中，这个 **`class`** 属性是一个识别名称，以后为元素设置样式信息时更加方便。

属性必须包含：

- 一个空格，它在属性和元素名称之间。如果一个元素具有多个属性，则每个属性之间必须由空格分隔。
- 属性名称，后面跟着一个等于号。
- 一个属性值，由一对引号（""）引起来。

### 布尔属性
有时你会看到没有值的属性，这也是完全可以接受的。这些属性被称为布尔属性。布尔属性只能有一个值，这个值一般与属性名称相同。例如，考虑 `disabled`属性，你可以将其分配给表单输入元素。用它来禁用表单输入元素，这样用户就不能输入了。被禁用的元素通常有一个灰色的外观。示例如下：

```html
<input type="text" disabled="disabled" />

<!-- 使用 disabled 属性来防止终端用户输入文本到输入框中 -->
<input type="text" disabled />

<!-- 下面这个输入框不包含 disabled 属性，所以用户可以向其中输入 -->
<input type="text" />

```

## HTML元信息
```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的测试页面</title>
  </head>
  <body>
    <p>这是我的页面</p>
  </body>
</html>

```

HTML 头部包含 HTML `<head>` 元素的内容，与 `<body>`元素内容不同，页面在浏览器加载后它的内容不会在浏览器中显示，它的作用是保存页面的一些**元数据**。元数据就是描述数据的数据，而 HTML 有一个“官方的”方式来为一个文档添加元数据——`<meta>` 元素。

```html
<meta charset="utf-8" />
```
这个元素简单的指定了文档的字符编码——在这个文档中被允许使用的字符集。


```html
<meta name="author" content="Chris Mills" />
<meta
  name="description"
  content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing web sites and applications." />

```

许多 `<meta>` 元素包含了 `name` 和 `content` 属性：

- `name` 指定了 meta 元素的类型；说明该元素包含了什么类型的信息。
- `content` 指定了实际的元数据内容。


## 文本处理
### 标题和段落
在 HTML 中，每个段落是通过 `<p>` 元素标签进行定义的，比如下面这样：
```html
<p>我是一个段落，千真万确。</p>
```

每个标题（Heading）都必须被包裹在一个标题元素中：
```html
<h1>我是文章的标题</h1>

```

一共有六种标题元素标签——[h1](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Heading_Elements)、[h2](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Heading_Elements)、[h3](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Heading_Elements)、[h4](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Heading_Elements)、[h5](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Heading_Elements) 和 [h6](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Heading_Elements)。每个元素代表文档中不同级别的内容：`<h1>` 表示主标题，`<h2>` 表示二级子标题，`<h3>` 表示三级子标题，依此类推。

