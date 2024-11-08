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


### 列表
在 Web 上，有三种类型的列表：无序列表、有序列表和描述列表。

#### 无序列表
无序列表用于标记列表项目顺序无关紧要的列表
```html
<ul>
  <li>豆浆</li>
  <li>油条</li>
  <li>豆汁</li>
  <li>焦圈</li>
</ul>
```

#### 有序列表
有序列表需要按照项目的顺序列出来
```html
<ol>
  <li>沿这条路走到头</li>
  <li>右转</li>
  <li>直行穿过第一个十字路口</li>
  <li>在第三个十字路口处左转</li>
  <li>继续走 300 米，学校就在你的右手边</li>
</ol>

```

#### 嵌套列表
将一个列表嵌入到另一个列表是完全可以的。
```html
<ol>
  <li>
    先用蛋白一个、盐半茶匙及淀粉两大匙搅拌均匀，调成“腌料”，鸡胸肉切成约一厘米见方的碎丁并用“腌料”搅拌均匀，腌渍半小时。
  </li>
  <li>
    用酱油一大匙、淀粉水一大匙、糖半茶匙、盐四分之一茶匙、白醋一茶匙、蒜末半茶匙调拌均匀，调成“综合调味料”。
  </li>
  <li>
    鸡丁腌好以后，色拉油下锅烧热，先将鸡丁倒入锅内，用大火快炸半分钟，炸到变色之后，捞出来沥干油汁备用。
  </li>
  <li>
    在锅里留下约两大匙油，烧热后将切好的干辣椒下锅，用小火炒香后，再放入花椒粒和葱段一起爆香。随后鸡丁重新下锅，用大火快炒片刻后，再倒入“综合调味料”继续快炒。
    <ul>
      <li>
        如果你采用正宗川菜做法，最后只需加入花生米，炒拌几下就可以起锅了。
      </li>
      <li>如果你在北方，可加入黄瓜丁、胡萝卜丁和花生米，翻炒后起锅。</li>
    </ul>
  </li>
</ol>
```

#### 描述列表
这种列表的目的是标记一组项目及其相关描述，例如术语和定义，或者是问题和答案等。
描述列表使用与其他列表类型不同的闭合标签——`<dl>`；此外，每一项都用 `<dt>`（description term）元素闭合。每个描述都用 `<dd>`（description definition）元素闭合。

```html
<dl>
  <dt>内心独白</dt>
  <dd>
    戏剧中，某个角色对自己的内心活动或感受进行念白表演，这些台词只面向观众，而其他角色不会听到。
  </dd>
  <dt>语言独白</dt>
  <dd>
    戏剧中，某个角色把自己的想法直接进行念白表演，观众和其他角色都可以听到。
  </dd>
  <dt>旁白</dt>
  <dd>
    戏剧中，为渲染幽默或戏剧性效果而进行的场景之外的补充注释念白，只面向观众，内容一般都是角色的感受、想法、以及一些背景信息等。
  </dd>
</dl>

```
### 强调
* `<em>`表示斜体
* `<strong>`表示粗体


```html
<p>我很<em>庆幸</em>你没有<em>迟到</em>。</p> 
<p>这杯液体<strong>毒性很大</strong>。</p>

<p>就指望你了，千万<strong>不要</strong>迟到！</p>

```

### 超链接
超链接是互联网提供的最令人兴奋的创新之一，它们从一开始就一直是互联网的一个特性，使互联网成为_互联的网络_。超链接使我们能够将我们的文档链接到任何其他文档（或其他资源），也可以链接到文档的指定部分，我们可以在一个简单的网址上提供应用程序。几乎任何网络内容都可以转换为链接，点击（或激活）超链接将使网络浏览器转到另一个网址（[URL](https://developer.mozilla.org/zh-CN/docs/Glossary/URL)）。

#### 链接的解析
通过将文本或其他内容包裹在 `<a>`元素内，并给它一个包含网址的 `href`属性（也称为**超文本引用**或**目标**，它将包含一个网址）来创建一个基本链接。

```html
<p>
  我创建了一个指向
  <a href="https://www.mozilla.org/zh-CN/">Mozilla 主页</a>的链接。
</p>

```

#### 块级链接
就像之前提到的那样，任何内容，甚至`块级元素`都可以作为链接出现。如果想让标题元素变为链接，就把它包裹在锚点元素（`<a>`）内，像这个代码段一样：
```html
<a href="https://developer.mozilla.org/zh-CN/">
  <h1>MDN Web 文档</h1>
</a>
<p>自从 2005 年起，就开始记载包括 CSS、HTML、JavaScript 等网络技术。</p>

```

#### 图片链接
如果有需要作为链接的图片，使用 `<a>` 元素来包裹要引用图片的 `<img>`元素。下面的示例使用相对路径来引用本地存储的 SVG 图像文件。
```html
<a href="https://developer.mozilla.org/zh-CN/">
  <img src="mdn_logo.svg" alt="MDN Web 文档" />
</a>

```

#### 使用 title 属性添加支持信息
你可能要添加到你的链接的另一个属性是 `title`。这旨在包含关于链接的补充信息，例如页面包含什么样的信息或需要注意的事情。
```html
<p>
  我创建了一个指向
  <a
    href="https://www.mozilla.org/zh-CN/"
    title="了解 Mozilla 使命以及如何参与贡献的最佳站点。">
    Mozilla 主页</a
  >的超链接。
</p>

```
当鼠标指针悬停在链接上时，标题将作为提示信息出现）

## 文档与网站架构

### 文档的基本组成部分
网页的外观多种多样，但是除了全屏视频或游戏，或艺术作品页面，或只是结构不当的页面以外，都倾向于使用类似的标准组件：

页眉
通常横跨于整个页面顶部有一个大标题 和/或 一个标志。这是网站的主要一般信息，通常存在于所有网页。

导航栏
指向网站各个主要区段的超链接。通常用菜单按钮、链接或标签页表示。类似于标题栏，导航栏通常应在所有网页之间保持一致，否则会让用户感到疑惑，甚至无所适从。

主内容
中心的大部分区域是当前网页大多数的独有内容，例如视频、文章、地图、新闻等。这些内容是网站的一部分，且会因页面而异。

侧边栏
一些外围信息、链接、引用、广告等。通常与主内容相关（例如一个新闻页面上，侧边栏可能包含作者信息或相关文章链接），还可能存在其他的重复元素，如辅助导航系统。

页脚
横跨页面底部的狭长区域。和标题一样，页脚是放置公共信息（比如版权声明或联系方式）的，一般使用较小字体，且通常为次要内容。还可以通过提供快速访问链接来进行 [SEO](https://developer.mozilla.org/zh-CN/docs/Glossary/SEO)。

### 用于构建内容的 HTML
HTML 代码中可根据功能来为区段添加标记。可使用元素来无歧义地表示上文所讲的内容区段，屏幕阅读器等辅助技术可以识别这些元素，并帮助执行“找到主导航“或”找到主内容“等任务。

为了实现语义化标记，HTML 提供了明确这些区段的专用标签，例如：

- `<header>`：页眉。
- `<nav>`：导航栏。
- `<main>`：主内容。主内容中还可以有各种子内容区段，可用`<article>`、`<section>` 和 `<div>` 等元素表示。
- `<aside>`：侧边栏，经常嵌套在 `<main>`中。
- `<footer>`：页脚。

```HTML
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>二次元俱乐部</title>
    <link
      href="https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Sonsie+One"
      rel="stylesheet" />
    <link
      href="https://fonts.googleapis.com/css?family=ZCOOL+KuaiLe"
      rel="stylesheet" />
    <link href="style.css" rel="stylesheet" />
  </head>

  <body>
    <header>
      <!-- 本站所有网页的统一主标题 -->
      <h1>聆听电子天籁之音</h1>
    </header>

    <nav>
      <!-- 本站统一的导航栏 -->
      <ul>
        <li><a href="#">主页</a></li>
        <!-- 共 n 个导航栏项目，省略…… -->
      </ul>

      <form>
        <!-- 搜索栏是站点内导航的一个非线性的方式。 -->
        <input type="search" name="q" placeholder="要搜索的内容" />
        <input type="submit" value="搜索" />
      </form>
    </nav>

    <main>
      <!-- 网页主体内容 -->
      <article>
        <!-- 此处包含一个 article（一篇文章），内容略…… -->
      </article>

      <aside>
        <!-- 侧边栏在主内容右侧 -->
        <h2>相关链接</h2>
        <ul>
          <li><a href="#">这是一个超链接</a></li>
          <!-- 侧边栏有 n 个超链接，略略略…… -->
        </ul>
      </aside>
    </main>

    <footer>
      <!-- 本站所有网页的统一页脚 -->
      <p>© 2050 某某保留所有权利</p>
    </footer>
  </body>
</html>

```

### HTML 布局元素细节
- `<main>` 存放每个页面独有的内容。每个页面上只能用一次 `<main>`，且直接位于 `<body>` 中。最好不要把它嵌套进其他元素。
- `<article>` 包围的内容即一篇文章，与页面其他部分无关（比如一篇博文）。
- `<section>`与 `<article>` 类似，但 `<section>` 更适用于组织页面使其按功能（比如迷你地图、一组文章标题和摘要）分块。一般的最佳用法是：以 标题作为开头；也可以把一篇 `<article>` 分成若干部分并分别置于不同的 `<section>` 中，也可以把一个区段 `<section>` 分成若干部分并分别置于不同的 `<article>` 中，取决于上下文。
- `<aside>` 包含一些间接信息（术语条目、作者简介、相关链接，等等）。
- `<header>` 是简介形式的内容。如果它是 `<body>` 的子元素，那么就是网站的全局页眉。如果它是 `<article>` 或`<section>` 的子元素，那么它是这些部分特有的页眉（此 `<header>` 非彼 标题）。
- `<nav>`包含页面主导航功能。其中不应包含二级链接等内容。
- `<footer>` 包含了页面的页脚部分。