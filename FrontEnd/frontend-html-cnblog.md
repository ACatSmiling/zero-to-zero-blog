>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-10`*

## HTML 4

**`HTML：HyperText Markup Language，超文本标记语言。`**

> 超文本的含义：是一种组织信息的方式，通过超链接将不同空间的文字、图片等各种信息组织在一起，能从当前阅读的内容，跳转到超链接所指向的内容，如页面、文件、锚点、应用等。

相关国际组织：

- IETF：Internet Engineering Task Force，国际互联网工程任务组。成立于 1985 年底，是一个权威的互联网技术标准化组织，主要负责互联网相关技术规范的研发和制定，当前绝大多数国际互联网技术标准均出自 IETF。官网：https://www.ietf.org。
- W3C：World Wide Web Consortium，万维网联盟。创建于 1994 年，是目前 Web 技术领域，最具影响力的技术标准机构。共计发布了 200 多项技术标准和实施指南，对互联网技术的发展和应用起到了基础性和根本性的支撑作用。官网：https://www.w3.org。
- WHATWF：Web Hypertext Application Technology Working Group，网页超文本应用技术工作小组。成立于 2004 年，是一个以推动网络 HTML 5 标准为目的而成立的组织。由 Opera、Mozilla 基金会、苹果，等这些浏览器厂商组成。官网：https://whatwg.org/。

HTML 发展历史：

![image-20231204223848137](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093857846-1252320730.png)

- 目前 HTML 的最新标准是 HMTL 5。

### 标签

**`标签`**：又称元素，是 HTML 的基本组成单位。

- 标签分为：双标签与单标签，绝大多数都是双标签。

  - 单标签：

    ![image-20231204224302800](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093857458-1310757380.png)

  - 双标签：

    ![image-20231204224237792](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093857104-1268945745.png)

- 标签名不区分大小写，但推荐小写，因为小写更规范。

- 标签之间的关系：并列关系、嵌套关系，可以使用 tab 键进行缩进。

### 标签属性

**`标签属性`**：用于给标签提供附加信息，可以写在**起始标签**或**单标签中**。

![image-20231204224553202](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093856751-229659914.png)

- 有些特殊的属性，没有属性名，只有属性值。例如：

  ```html
  <input disabled>
  ```

- 规范：

  - 不同的标签，有不同的属性；也有一些通用属性，在任何标签内都能写。

  - 属性名、属性值不能乱写，都是 W3C 规定好的。

  - 属性名、属性值，都不区分大小写，但推荐小写。

  - 双引号，也可以写成单引号，甚至不写都行，但还是推荐写双引号。

  - 标签中不要出现同名属性，否则后写的会失效。例如：

    ```html
    <input type="text" type="password">
    ```

### 基本结构

在网页中，通过点击鼠标右键，选择 "检查"，可以查看某段结构的具体代码。

"检查" 和 "查看网页源代码" 的区别：

- "检查" 看到的是：经过浏览器处理后的源代码。
- "查看网页源代码" 看到的是：程序员编写的源代码。

网页的`基本结构`如下：

- 想要呈现在网页中的内容写在 body 标签中。

- head 标签中的内容不会出现在网页中。

- head 标签中的 title 标签可以指定网页的标题。

- 图示：

  ![image-20231204225242206](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093856341-3911652.png)

- 代码：

  ```html
  <html>
      <head>
      	<title>网页标题</title>
      </head>
      
      <body>
      	......
      </body>
  </html>
  ```

### 注释

特点：注释的内容会被浏览器所忽略，不会呈现到页面中，但源代码中依然可见。

作用：对代码进行解释和说明。

写法：

```html
<!-- 下面的文字只能滚动一次 -->
<marquee loop="1">滚动一次</marquee>

<!-- 下面的文字可以无限滚动 -->
<marquee>无限滚动123</marquee>
```

注释不可以嵌套，以下写法是错的：

```html
<!--
	我是一段注释
	<!-- 我是一段注释 -->
-->
```

### 文档声明

作用：告诉浏览器当前网页的版本。

写法（W3C 推荐使用 HTML 5 的写法）：

```html
<!DOCTYPE html>
或
<!DOCTYPE HTML>
或
<!doctype html>
```

> 注意：文档声明，必须在网页的第一行，且在 html 标签的外侧。

### 字符编码

平时编写代码时，统一采用 UTF-8 编码。为了让浏览器在渲染 html 文件时，不犯错误，可以通过 meta 标签配合 charset 属性指定字符编码：

```html
<head>
	<meta charset="UTF-8"/>
</head>
```

### 设置语言

主要作用：

- 让浏览器显示对应的翻译提示。
- 有利于搜索引擎优化。

写法：

```html
<html lang="zh-CN">
```

> 扩展知识： lang 属性的编写规则。
>
> 1. 第一种写法（ 语言-国家/地区 ），例如：
>
>    - zh-CN ：中文-中国大陆（简体中文）。
>    - zh-TW ：中文-中国台湾（繁体中文）。
>    - zh ：中文。
>    - en-US ：英语-美国。
>    - en-GB ：英语-英国。
>
> 2. 第二种写法（ 语言—具体种类），已不推荐使用，例如：
>
>    - zh-Hans ：中文—简体。
>
>      zh-Hant ：中文—繁体。
>
> 3. W3School 上的说明：[《语言代码参考手册》](https://www.w3school.com.cn/tags/html_ref_language_codes.asp)、[《国家/地区代码参考手册》](https://www.w3school.com.cn/tags/html_ref_country_codes.asp)。
>
> 4. W3C 官网上的说明：[《Language tags in HTML》](https://www.w3.org/International/articles/language-tags/)。

### 标准结构

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">
    <head>
        <meta charset="UTF-8">
        <title>我是一个标题</title>
    </head>
    <body>
    </body>
</html>
```

>输入`!`，随后回车即可快速生成标准结构。

使用 ! 符号，默认生成的是 lang="en"，可以通过以下设置：

![image-20231205220032270](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093855977-2043813148.png)

在存放代码的文件夹中，存放一个 favicon.ico 图片，可配置网站图标。示例：

![image-20231205220755477](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093855578-1705093205.png)

> 注意：添加图片之后，可能需要清空缓存，重新加载页面。

### 排版标签

| 标签名    | 标签语义                   | 单/双标签 |
| --------- | -------------------------- | --------- |
| `h1 ~ h6` | 标题                       | 双        |
| `p`       | 段落                       | 双        |
| `div`     | 没有任何含义，用于整体布局 | 双        |

- h1 最好写一个， h2 ~ h6 能适当多写。
- h1 ~ h6 不能互相嵌套。例如：h1 标签中不要写 h2 标签。
- p 标签很特殊！它里面不能有：h1 ~ h6、p、div 标签。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>HTML排版标签</title>
</head>

<body>
    <!-- 
        <h1>我是一级标题</h1>
        <h2>我是二级标题</h2>
        <h3>我是三级标题</h3>
        <h4>我是四级标题</h4>
        <h5>我是五级标题</h5>
        <h6>我是六级标题</h6>
        <div>
            <p>我是一个段落</p>
            <p>我是一个段落</p>
            <p>我是一个段落</p>
        </div>
     -->
    <h1>把个人信息“安全堤”筑牢</h1>
    <h4>2022-06-21 07:34 · 1347条评论</h4>
    <div>
        <p>置身移动互联时代，人们在享受智能设备带来便利的同时，也在一些场景中面临个人信息泄露风险。随着时间推移，这样的风险日益呈现出新的表现形式。</p>
        <p>一些APP声称看视频、玩游戏甚至走路都能赚钱，但用户想提现却难上加难，还容易泄露个人信息；新型不法软件图标透明、没有名称，在手机屏幕上十分隐蔽，不仅不停推送广告，还会收集并转卖用户隐私信息；个人信息安全和隐私保护话题引发关注，正说明其涉及群众切身利益，问题不容小视。
        </p>
    </div>
</body>

</html>
```

### 语义化标签

概念：用特定的标签，去表达特定的含义。

原则：**标签的默认效果不重要，语义最重要！**（后期可以通过 CSS 随便控制默认效果）

举例：对于 h1 标签，效果是文字很大（不重要），语义是网页主要内容（很重要）。

优点：

- 代码结构清晰可读性强。
- 有利于 SEO（搜索引擎优化）。
- 方便设备解析，如屏幕阅读器、盲人阅读器等。

### 块级元素与行内元素

`块级元素`：独占一行（排版标签都是块级元素）。

`行内元素`：不独占一行，例如： input。

使用原则：

- 块级元素中能写行内元素和块级元素（简单记：块级元素中几乎什么都能写）。
- 行内元素中能写行内元素，但不能写块级元素。
- 一些特殊的规则：
  - h1 ~ h6 不能互相嵌套。
  - p 中不要写块级元素。

>Tips：
>
>marquee 元素设计的初衷是：让文字有动画效果，但如今可以通过 CSS 来实现，而且还可以实现的更加炫酷，所以 marquee 标签已经过时了（废弃了），不推荐使用。

### 文本标签

作用：用于包裹词汇、短语等，通常写在排版标签里面。文本标签通常都是行内元素。

>排版标签更宏观（大段的文字），文本标签更微观（词汇、短语）。

常用：

| 标签名   | 标签语义                         | 单/双标签 |
| -------- | -------------------------------- | --------- |
| `em`     | 要着重阅读的内容                 | 双        |
| `strong` | 十分重要的内容，语气比 em 要强   | 双        |
| `span`   | 没有语义，用于包裹短语的通用容器 | 双        |

不常用：

| 标签名     | 标签语义                                                     | 单/双标签 |
| ---------- | ------------------------------------------------------------ | --------- |
| cite       | 作品标题（书籍、歌曲、电影、电视节目、绘画、雕塑）           | 双        |
| dfn        | 特殊术语 ，或专属名词                                        | 双        |
| del 与 ins | 删除的文本【与】插入的文本                                   | 双        |
| sub 与 sup | 下标文字【与】上标文字                                       | 双        |
| code       | 一段代码                                                     | 双        |
| samp       | 从正常的上下文中，将某些内容提取出来，例如：标识设备输出     | 双        |
| kbd        | 键盘文本，表示文本是通过键盘输入的，经常用在与计算机相关的手册中 | 双        |
| abbr       | 缩写，最好配合上 title 属性                                  | 双        |
| bdo        | 更改文本方向，要配合 dir 属性，可选值: ltr（默认值）、rtl。ltr 表示从左往右，rtl 表示从右往左。 | 双        |
| var        | 标记变量，可以与 code 标签一起使用                           | 双        |
| small      | 附属细则，例如：包括版权、法律文本。—— 很少使用              | 双        |
| b          | 摘要中的关键字、评论中的产品名称。—— 很少使用                | 双        |
| i          | 本意是：人物的思想活动、所说的话等等。现在多用于：呈现字体图标。 | 双        |
| u          | 与正常内容有反差文本，例如：错的单词、不合适的描述等。—— 很少使用 | 双        |
| q          | 短引用 —— 很少使用                                           | 双        |
| blockquote | 长引用 —— 很少使用                                           | 双        |
| address    | 地址信息                                                     | 双        |

- 这些不常用的文本标签，编码时不用过于纠结，酌情而定，不用也没毛病。
- blockquote 与 address 是块级元素，其他的文本标签，都是行内元素。
- 有些语义感不强的标签，我们很少使用，例如：small 、 b 、 u 、 q 、 blockquote。
- HTML 标签太多了！记住那些：重要的、语义感强的标签即可。截止目前，有这些：h1 ~ h6 、 p 、 div 、 em 、 strong 、 span。

### 图片标签

| 标签名 | 标签语义 | 常用属性                                                     | 单/双标签 |
| ------ | -------- | ------------------------------------------------------------ | --------- |
| `img`  | 图片     | `src`：图片路径，又称图片地址，指图片的具体位置。<br />`alt`：图片描述。<br />width：图片宽度，单位是像素，例如： 200 px 或 200。<br />height：图片高度， 单位是像素，例如： 200 px 或 200。 | 单        |

- `px`表示像素，是一种单位。
- 尽量不同时修改图片的宽和高，可能会造成比例失调。
- 暂且认为 img 是行内元素。（学到 CSS 时，会认识一个新的元素分类，目前只知道：块级元素和行内元素）
- `alt`属性的作用：
  - **搜索引擎可以通过 alt 属性，得知图片的内容，这是最主要的作用。**
  - 当图片无法展示时候，有些浏览器会呈现 alt 属性的值。
  - 盲人阅读器会朗读 alt 属性的值。

> `相对路径`：以**当前位置**作为参考点，去建立路径。
>
> - `./`：同级，可以省略不写。
> - `/`：下一级。
> - `../`：上一级。
> - 相对路径依赖的是当前位置，后期若调整了文件位置，那么文件中的路径也要修改。
>
> `绝对路径`：以**根位置**作为参考点，去建立路径。
>
> - 本地绝对路径：E:/a/b/c/奥特曼.jpg。
> - 网络绝对路径：http://www.atguigu.com/images/index_new/logo.png。
> - 使用本地绝对路径，一旦更换设备，路径处理起来比较麻烦，所以很少使用。
> - 使用网络绝对路径，确实方便，但要注意：若服务器开启了防盗链，会造成图片引入失败。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>HTML_图片标签</title>
</head>

<body>
    <img width="200" src="奥特曼.jpg" alt="奥特曼，你相信光吗？">
    <img width="200" src="奥特曼2.jpg" alt="奥特曼，你相信光吗？">
</body>

</html>
```

**常见图片格式：**

- `jpg`格式：

  - 概述：扩展名为`.jpg`或`.jpeg`，是一种**有损的压缩格式**（把肉眼不容易观察出来的细节丢弃了）。
  - 主要特点：**支持的颜色丰富、占用空间较小**、不支持透明背景、不支持动态图。
  - 使用场景：对图片细节**没有极高要求**的场景。例如：网站的产品宣传图等 。（该格式在网页中很常见）

- `png`格式：

  - 概述：扩展名为`.png`，是一种**无损的压缩格式**，能够更高质量的保存图片。
  - 主要特点：**支持的颜色丰富**、占用空间略大、**支持透明背景**、不支持动态图。
  - 使用场景：想让图片有透明背景；想更高质量的呈现图片。例如：公司 logo 图、重要配图等。

- `bmp`格式：

  - 概述：扩展名为`.bmp`，**不进行压缩的一种格式**，在最大程度上保留图片更多的细节。
  - 主要特点：**支持的颜色丰富、保留的细节更多**、占用空间极大、不支持透明背景、不支持动态图。
  - 使用场景：对图片细节**要求极高**的场景。例如：一些大型游戏中的图片 。（网页中很少使用）

- `gif`格式：

  - 概述：扩展名为`.gif`，仅支持 256 种颜色，色彩呈现不是很完整。
  - 主要特点：支持的颜色较少、**支持简单透明背景、支持动态图**。
  - 使用场景：网页中的动态图片。

- `webp`格式：

  - 概述：扩展名为`.webp`，谷歌推出的一种格式，专门用来在网页中呈现图片。
  - 主要特点：具备上述几种格式的优点，但兼容性不太好，一旦使用务必要解决兼容性问题。
  - 使用场景：网页中的各种图片。

- `base64`格式：

  - 本质：一串特殊的文本，要通过浏览器打开，传统看图应用通常无法打开。

  - **原理：把图片进行 base64 编码，形成一串文本。**

  - 如何生成：靠一些工具或网站。

  - 如何使用：直接作为 img 标签的 src 属性的值即可，并且不受文件位置的影响。

  - 使用场景：一些较小的图片，或者需要和网页一起加载的图片。

  - 示例：

    ```html
    <!DOCTYPE html>
    <html lang="zh-CN">
    
    <head>
        <meta charset="UTF-8">
        <title>演示base64图片</title>
    </head>
    
    <body>
        <img src="./奥特曼.jpg" alt="">
        <img src="./path_test/怪兽.jpg" alt="">
    
        <!-- base64图片 -->
        <img src="data:image/png;base64,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"
            alt="">
    </body>
    
    </html>
    ```

> 图片的格式非常多，上面这些，只是一些常见的、前端人员常接触到的。

### 超链接

| 标签名 | 标签语义 | 常用属性                                                     | 单/双标签 |
| ------ | -------- | ------------------------------------------------------------ | --------- |
| `a`    | 超链接   | `href`：指定要跳转到的具体目标。<br />`target`：控制跳转时如何打开页面。常用值如下：<br />                        `_self`：在本窗口打开，默认值。<br />                        `_blank`：在新窗口打开。<br />`id`：元素的唯一 标识，可用于设置锚点。<br />`name`：元素的名字，写在 a 标签中，也能设置锚点。 | 双        |

主要作用：**从当前页面进行跳转**。

可以实现：①跳转到指定页面、②跳转到指定文件（也可触发下载）、③跳转到锚点位置、④唤起指定应用。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>超链接_跳转页面</title>
</head>

<body>
    <a href="https://miaosha.jd.com/" target="_self">去秒杀</a>
    <a href="https://www.baidu.com/" target="_blank">去百度</a>
    <a href="./10_HTML排版标签.html">去排版标签</a>

    <a href="./10_HTML排版标签.html">
        <img width="50" src="./奥特曼.jpg" alt="奥特曼">
    </a>
</body>

</html>
```

#### 跳转到页面

示例：

```html
<!-- 跳转其他网页 -->
<a href="https://www.jd.com/" target="_blank">去京东</a>

<!-- 跳转本地网页 -->
<a href="./10_HTML排版标签.html" target="_self">去看排版标签</a>
```

> 注意：
>
> - 代码中的**多个空格**、**多个回车**，都会被浏览器解析成一个空格！
> - 虽然 a 标签是行内元素，但 a 标签可以包裹除它自身外的任何元素！

#### 跳转到文件

示例：

```html
<!-- 浏览器能直接打开的文件 -->
<a href="./resource/自拍.jpg">看自拍</a>
<a href="./resource/小电影.mp4">看小电影</a>
<a href="./resource/小姐姐.gif">看小姐姐</a>
<a href="./resource/如何一夜暴富.pdf">点我一夜暴富</a>

<!-- 浏览器不能打开的文件，会自动触发下载 -->
<a href="./resource/内部资源.zip">内部资源</a>

<!-- 强制触发下载 -->
<a href="./resource/小电影.mp4" download="电影片段.mp4">下载电影</a>
```

>注意：
>
>- 若浏览器无法打开文件，则会引导用户下载。
>- 若希望强制触发下载，请使用`download`属性，属性值即为下载文件的名称。

#### 跳转到锚点

`锚点`：网页中的一个标记点。

使用方式：

1. **第一步：设置锚点。**

   ```html
   <!-- 第一种方式：a标签配合name属性 -->
   <a name="test1"></a>
   
   <!-- 第二种方式：其他标签配合id属性，推荐 -->
   <h2 id="test2">我是一个位置</h2>
   ```

   - **具有 href 属性的 a 标签是超链接，具有 name 属性的 a 标签是锚点。**
   - name 和 id 都是区分大小写的，且 id 最好别是数字开头。

2. **第二步：跳转锚点。**

   ```html
   <!-- 跳转到test1锚点，#不可省略 -->
   <a href="#test1">去test1锚点</a>
   
   <!-- 跳到本页面顶部，页面没有变化，滚动条拉到顶部 -->
   <a href="#">回到顶部</a>
   
   <!-- 跳转到其他页面锚点 -->
   <a href="demo.html#test1">去demo.html页面的test1锚点</a>
   
   <!-- 刷新本页面，页面刷新，然后回到顶部 -->
   <a href="">刷新本页面</a>
   
   <!-- 执行一段js (在:和;中间书写js脚本)，如果还不知道执行什么，可以留空：javascript:; -->
   <a href="javascript:alert(1);">点我弹窗</a>
   ```

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>超链接_跳转锚点</title>
</head>

<body>
    <a href="#htl">看灰太狼</a>
    <a href="#atm">看奥特曼</a>

    <p>我是一只羊，一只很肥美的羊</p>
    <img src="./path_test/a/喜羊羊.jpg" alt="喜羊羊">

    <a name="htl"></a>
    <p>我是一只狼，一只很邪恶的狼</p>
    <img src="./path_test/a/b/灰太狼.jpg" alt="灰太狼">

    <p id="atm">我是一只奥特曼，一只很能打的奥特曼</p>
    <img src="./奥特曼.jpg" alt="奥特曼">

    <p>我是一只怪兽，一只很丑的怪兽</p>
    <img src="./path_test/怪兽.jpg" alt="怪兽">

    <p>整体的介绍完毕了</p>
    <a href="#">回到顶部</a>
    <a href="">刷新页面</a>
    <a href="javascript:;">点我弹窗</a>
</body>

</html>
```

#### 唤起指定应用

通过 a 标签，可以唤起设备上的应用程序。

```html
<!-- 唤起设备拨号 -->
<a href="tel:10010">电话联系</a>

<!-- 唤起设备发送邮件 -->
<a href="mailto:10010@qq.com">邮件联系</a>

<!-- 唤起设备发送短信 -->
<a href="sms:10086">短信联系</a>
```

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>超链接_唤起指定应用</title>
</head>

<body>
    <a href="tel:10010">电话联系</a>
    <a href="mailto:1234567@qq.com">邮件联系</a>
    <a href="sms:10086">短信联系</a>
</body>

</html>
```

### 列表

#### 有序列表

概念：有顺序或侧重顺序的列表。

示例：

```html
<h2>要把大象放冰箱总共分几步</h2>
<ol>
    <li>把冰箱门打开</li>
    <li>把大象放进去</li>
    <li>把冰箱门关上</li>
</ol>
```

#### 无序列表

概念：无顺序或不侧重顺序的列表。

示例：

```html
<h2>我想去的几个城市</h2>
<ul>
    <li>成都</li>
    <li>上海</li>
    <li>西安</li>
    <li>武汉</li>
</ul>
```

#### 嵌套列表

概念：列表中的某项内容，又包含一个列表（注意：嵌套时，请务必把解构写完整）。

示例：

```html
<h2>我想去的几个城市</h2>
<ul>
    <li>成都</li>
    <li>
        <span>上海</span>
        <ul>
            <li>外滩</li>
            <li>杜莎夫人蜡像馆</li>
            <li>
            	<a href="https://www.opg.cn/">东方明珠</a>
            </li>
            <li>迪士尼乐园</li>
    	</ul>
    </li>
    <li>西安</li>
    <li>武汉</li>
</ul>
```

> 注意：`li 标签最好写在 ul 或 ol 中，不要单独使用。`

#### 自定义列表

概念：所谓自定义列表，就是一个`包含术语名称以及术语描述的列表`。

示例：

```html
<h2>如何高效的学习？</h2>
<dl>
    <dt>做好笔记</dt>
    <dd>笔记是我们以后复习的一个抓手</dd>
    <dd>笔记可以是电子版，也可以是纸质版</dd>
    <dt>多加练习</dt>
    <dd>只有敲出来的代码，才是自己的</dd>
    <dt>别怕出错</dt>
    <dd>错很正常，改正后并记住，就是经验</dd>
</dl>
```

- 一个`dl`就是一个自定义列表，一个`dt`就是一个术语名称，一个`dd`就是术语描述（可以有多个）。

### 表格

#### 基本结构

一个完整的表格由：**表格标题**、**表格头部**、**表格主体**、**表格脚注**，四部分组成。

![image-20231208224628843](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093855235-1665335607.png)

表格涉及到的标签：

- `tabl`：表格。

- `caption`：表格标题。

- `thead`：表格头部。

- `tr`：每一行。

- `th`、`td`：每一个单元格。（备注：表格头部中用 th ，表格主体、表格脚注中用 td。）

  ![image-20231208224857966](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093854909-1689669308.png)

  ![image-20231208225024172](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093854537-1263529556.png)

  ![image-20231208225041298](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093854203-1354133349.png)

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表格_整体结构</title>
</head>

<body>
    <table border="1">
        <!-- 表格标题 -->
        <caption>学生信息</caption>
        <!-- 表格头部 -->
        <thead>
            <tr>
                <th>姓名</th>
                <th>性别</th>
                <th>年龄</th>
                <th>民族</th>
                <th>政治面貌</th>
            </tr>
        </thead>
        <!-- 表格主体 -->
        <tbody>
            <tr>
                <td>张三</td>
                <td>男</td>
                <td>18</td>
                <td>汉族</td>
                <td>团员</td>
            </tr>
            <tr>
                <td>李四</td>
                <td>女</td>
                <td>20</td>
                <td>满族</td>
                <td>群众</td>
            </tr>
            <tr>
                <td>王五</td>
                <td>男</td>
                <td>19</td>
                <td>回族</td>
                <td>党员</td>
            </tr>
            <tr>
                <td>赵六</td>
                <td>女</td>
                <td>21</td>
                <td>壮族</td>
                <td>团员</td>
            </tr>
        </tbody>
        <!-- 表格脚注 -->
        <tfoot>
            <tr>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
                <td>共计：4人</td>
            </tr>
        </tfoot>
    </table>
</body>

</html>
```

#### 常用属性

| 标签名  | 标签语义   | 常用属性                                                     | 单/双标签 |
| ------- | ---------- | ------------------------------------------------------------ | --------- |
| `table` | 表格       | `width`：设置表格宽度。<br />`height`：设置表格**最小**高度，表格最终高度可能比设置的值大。<br />`border`：设置表格边框宽度。<br />`cellspacing`： 设置单元格之间的间距。 | 双        |
| `thead` | 表格头部   | `height`：设置表格头部高度。<br />`align`： 设置单元格的**水平**对齐方式，可选值如下：<br />                `left`：左对齐<br />                `center`：中间对齐<br />                `right`：右对齐<br />`valign`：设置单元格的**垂直**对齐方式，可选值如下：<br />                `top`：顶部对齐<br />                `middle`：中间对齐<br />                `bottom`：底部对齐 | 双        |
| `tbody` | 表格主体   | 常用属性与 thead 相同。                                      | 双        |
| `tfoot` | 表格脚注   | 常用属性与 thead 相同。                                      | 双        |
| `tr`    | 行         | 常用属性与 thead 相同。                                      | 双        |
| `td`    | 普通单元格 | `width`：设置单元格的宽度，同列所有单元格全都受影响。<br />`heigth`：设置单元格的高度，同行所有单元格全都受影响。<br />`align`：设置单元格的水平对齐方式。<br />`valign`：设置单元格的垂直对齐方式。<br />`rowspan`：指定要跨的行数。<br />`colspan`：指定要跨的列数。 | 双        |
| `th`    | 表头单元格 | 常用属性与 td 相同。                                         | 双        |

- `<table>`元素的 border 属性可以控制表格边框，但 border 值的大小，并不控制单元格边框的宽度，只能控制表格最外侧边框的宽度，这个问题如何解决？—— 后期靠 CSS 控制。
- 默认情况下，每列的宽度，得看这一列单元格最长的那个文字。
- 给某个 th 或 td 设置了宽度之后，它们所在的那一列的宽度就确定了。
- 给某个 th 或 td 设置了高度之后，它们所在的那一行的高度就确定了。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表格_常用属性</title>
</head>

<body>
    <table border="1" width="500" height="500" cellspacing="0">
        <!-- 表格标题 -->
        <caption>学生信息</caption>
        <!-- 表格头部 -->
        <thead height="50" align="center" valign="middle">
            <tr>
                <th width="50" height="50" align="right" valign="bottom">姓名</th>
                <th>性别</th>
                <th>年龄</th>
                <th>民族</th>
                <th>政治面貌</th>
            </tr>
        </thead>
        <!-- 表格主体 -->
        <tbody height="520" align="center" valign="middle">
            <tr height="50" align="center" valign="middle">
                <td>张三</td>
                <td>男</td>
                <td>18</td>
                <td>汉族</td>
                <td>团员</td>
            </tr>
            <tr>
                <td>李四</td>
                <td>女</td>
                <td>20</td>
                <td>满族</td>
                <td>群众</td>
            </tr>
            <tr>
                <td>王五</td>
                <td>男</td>
                <td>19</td>
                <td>回族</td>
                <td>党员</td>
            </tr>
            <tr>
                <td align="right" valign="top">赵六</td>
                <td>女</td>
                <td>21</td>
                <td>壮族</td>
                <td>团员</td>
            </tr>
        </tbody>
        <!-- 表格脚注 -->
        <tfoot height="50" align="center" valign="middle">
            <tr>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
                <td>共计：4人</td>
            </tr>
        </tfoot>
    </table>
</body>

</html>
```

#### 跨行跨列

标签：

- `rowspan`：指定要跨的行数。
- `colspan`：指定要跨的列数。

示例，课程表效果图：

![image-20231208230338754](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093853810-1690846236.png)

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表格_跨行与跨列</title>
</head>

<body>
    <table border="1" cellspacing="0">
        <caption>课程表</caption>
        <thead>
            <tr>
                <th>项目</th>
                <th colspan="5">上课</th>
                <th colspan="2">活动与休息</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>星期</td>
                <td>星期一</td>
                <td>星期二</td>
                <td>星期三</td>
                <td>星期四</td>
                <td>星期五</td>
                <td>星期六</td>
                <td>星期日</td>
            </tr>
            <tr>
                <td rowspan="4">上午</td>
                <td>3-2</td>
                <td>3-3</td>
                <td>3-4</td>
                <td>3-5</td>
                <td>3-6</td>
                <td>3-7</td>
                <td rowspan="4">休息</td>
            </tr>
            <tr>
                <td>4-2</td>
                <td>4-3</td>
                <td>4-4</td>
                <td>4-5</td>
                <td>4-6</td>
                <td>4-7</td>
            </tr>
            <tr>
                <td>5-2</td>
                <td>5-3</td>
                <td>5-4</td>
                <td>5-5</td>
                <td>5-6</td>
                <td>5-7</td>
            </tr>
            <tr>
                <td>6-2</td>
                <td>6-3</td>
                <td>6-4</td>
                <td>6-5</td>
                <td>6-6</td>
                <td>6-7</td>
            </tr>
            <tr>
                <td rowspan="2">下午</td>
                <td>7-2</td>
                <td>7-3</td>
                <td>7-4</td>
                <td>7-5</td>
                <td>7-6</td>
                <td>7-7</td>
                <td rowspan="2">休息</td>
            </tr>
            <tr>
                <td>8-2</td>
                <td>8-3</td>
                <td>8-4</td>
                <td>8-5</td>
                <td>8-6</td>
                <td>8-7</td>
            </tr>
        </tbody>
    </table>
</body>

</html>
```

### 其他常用标签

| 标签名 | 标签语义                                 | 单/双标签 |
| ------ | ---------------------------------------- | --------- |
| `br`   | 换行                                     | 单        |
| `hr`   | 分隔                                     | 单        |
| `pre`  | 按原文显示，一般用于在页面中嵌入大段代码 | 双        |

- 不要用`<br>`来增加文本之间的行间隔，应使用`<p>`元素，或后面即将学到的 CSS margin 属性。
- `<hr>`的语义是分隔，如果不想要语义，只是想画一条水平线，那么应当使用 CSS 完成。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>补充几个常用的标签</title>
</head>

<body>
    <!-- 换行标签 -->
    <a href="https://www.baidu.com">去百度</a>
    <br>
    <a href="https://www.jd.com">去京东</a>
    <!-- 分割线 -->
    <div>第一章</div>
    <p>xxxxxx，就这样最后王子和公主就在一起了！</p>
    <hr>
    <div>第二章</div>
    <p>一个月黑风高的晚上，xxxxxxxxxxxxxx</p>
    <!-- 按原文显示 -->
    <pre>
        I      Love      You
           I   Love   You
               Love
    </pre>
</body>

</html>
```

> pre 元素中的内容，多个空格都保留了原样。

### 表单

概念：一个包含交互的区域，用于收集用户提供的数据。

#### 基本结构

| 标签名   | 标签语义 | 常用属性                                                     | 单/双标签 |
| -------- | -------- | ------------------------------------------------------------ | --------- |
| `form`   | 表单     | `action`：用于指定表单的提交地址（需要与后端人员沟通后确定）<br />`target`：用于控制表单提交后，如何打开页面，常用值如下：<br />                        `_self`：在本窗口打开，默认值<br />                        `_blank`：在新窗口打开<br />`method`：用于控制表单的提交方式，暂时只需了解，详见`Ajax` | 双        |
| `input`  | 输入框   | `type`：设置输入框的类型，目前用到的值是 text ，表示普通文本<br />`name`：用于指定提交数据的名字（需要与后端人员沟通后确认） | 单        |
| `button` | 按钮     | 详见后文                                                     | 双        |

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表单_基本结构</title>
</head>

<body>
    <form action="https://www.baidu.com/s">
        <input type="text" name="wd">
        <button>去百度搜索</button>
    </form>
    <hr>
    <form action="https://search.jd.com/search" target="_self" method="get">
        <input type="text" name="keyword">
        <button>去京东搜索</button>
    </form>
    <hr>
    <a href="https://search.jd.com/search?keyword=手机">搜索手机</a>
</body>

</html>
```

> `input`标签的 name 属性值，对应后台请求参数名，不能随意定义。

#### 常用表单控件

##### 文本输入框

```html
<input type="text">
```

常用属性：

- `name`属性：数据的名称。

- `value`属性：输入框的默认输入值。

- `maxlength`属性：输入框最大可输入长度。

##### 密码输入框

```html
<input type="password">
```

常用属性：

- `name`属性：数据的名称。
- `value`属性：输入框的默认输入值（一般不用，无意义）。
- `maxlength`属性：输入框最大可输入长度。

##### 单选框

```html
<input type="radio" name="sex" value="female">女
<input type="radio" name="sex" value="male">男
```

常用属性：

- `name`属性：数据的名称。**注意：想要单选效果，多个 radio 的 name 属性值要保持一致。**
- `value`属性：提交的数据值。
- `checked`属性：让该单选按钮默认选中。

##### 复选框

```html
<input type="checkbox" name="hobby" value="smoke">抽烟
<input type="checkbox" name="hobby" value="drink">喝酒
<input type="checkbox" name="hobby" value="perm">烫头
```

常用属性：

- `name`属性：数据的名称。
- `value`属性：提交的数据值。
- `checked`属性：让该复选框默认选中。

##### 文本域

```html
<textarea name="msg" rows="22" cols="3">我是文本域</textarea>
```

常用属性：

- `rows`属性：指定默认显示的行数，会影响文本域的高度。
- `cols`属性：指定默认显示的列数，会影响文本域的宽度。
- 不能编写 type 属性，其他属性，与普通文本输入框一致。

##### 下拉框

```html
<select name="from">
    <option value="黑">黑龙江</option>
    <option value="辽">辽宁</option>
    <option value="吉">吉林</option>
    <option value="粤" selected>广东</option>
</select>
```

常用属性：

- `name`属性：指定数据的名称。
- `option`标签设置`value`属性， 如果没有 value 属性，提交的数据是 option 中间的文字；如果设置了 value 属性，提交的数据就是 value 的值（建议设置 value 属性）。
- option 标签设置了`selected`属性，表示默认选中。

##### 隐藏域

```html
<input type="hidden" name="tag" value="100">
```

用户不可见的一个输入区域，作用是： 提交表单的时候，携带一些固定的数据。

- `name`属性：指定数据的名称。

- `value`属性：指定的是真正提交的数据。

##### 提交按钮

```html
<input type="submit" value="点我提交表单">

<button>点我提交表单</button>
```

注意：

- button 标签 type 属性的默认值是`submit`。
- button 不要指定 name 属性。（input 方式也不要指定 name 属性）
- input 标签编写的按钮，使用 value 属性指定按钮文字。

##### 重置按钮

```html
<input type="reset" value="点我重置">

<button type="reset">点我重置</button>
```

注意：

- button 不要指定 name 属性。（input 方式也不要指定 name 属性）
- input 标签编写的按钮，使用 value 属性指定按钮文字。

##### 普通按钮

```html
<input type="button" value="普通按钮">

<button type="button">普通按钮</button>
```

注意：

- 普通按钮的 type 值为 button，若不写，**button 元素的 type 属性值默认是 submit**，点击会引起表单的提交。

##### 示例

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表单_常用控件</title>
</head>

<body>
    <form action="https://search.jd.com/search">
        <!-- 文本输入框 -->
        账户：<input type="text" name="account" value="zhangsan" maxlength="10"><br>
        <!-- 密码输入框 -->
        密码：<input type="password" name="pwd" value="123" maxlength="6"><br>
        <!-- 单选框 -->
        性别：
        <input type="radio" name="gender" value="male">男
        <input type="radio" name="gender" value="female" checked>女<br>
        <!-- 多选框 -->
        爱好：
        <input type="checkbox" name="hobby" value="smoke" checked>抽烟
        <input type="checkbox" name="hobby" value="drink">喝酒
        <input type="checkbox" name="hobby" value="perm" checked>烫头<br>
        <!-- 文本域 -->
        其他：
        <textarea name="other" cols="23" rows="3"></textarea><br>
        <!-- 下拉框 -->
        籍贯：
        <select name="place">
            <option value="冀">河北</option>
            <option value="鲁">山东</option>
            <option value="晋" selected>山西</option>
            <option value="粤">广东</option>
        </select>
        <!-- 隐藏域 -->
        <input type="hidden" name="from" value="toutiao">
        <br>
        <!-- 确认按钮_第一种写法 -->
        <button type="submit">确认</button>
        <!-- 确认按钮_第二种写法 -->
        <!-- <input type="submit" value="确认"> -->
        <!-- 重置按钮_第一种写法 -->
        <!-- <button type="reset">重置</button> -->
        <!-- 重置按钮_第二种写法 -->
        <input type="reset" value="点我重置">
        <!-- 普通按钮_第一种写法 -->
        <input type="button" value="检测账户是否被注册">
        <!-- 普通按钮_第二种写法 -->
        <!-- <button type="button">检测账户是否被注册</button> -->
    </form>
</body>

</html>
```

#### 禁用表单控件

给表单控件的标签设置`disabled`，既可禁用表单控件。

>input、textarea、button、select、option 都可以设置 disabled 属性。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表单_禁用表单控件</title>
</head>

<body>
    <form action="https://search.jd.com/search">
        <!-- 文本输入框 -->
        账户：<input disabled type="text" name="account" value="zhangsan" maxlength="10"><br>
        <!-- 密码输入框 -->
        密码：<input type="password" name="pwd" value="123" maxlength="6"><br>
        <!-- 单选框 -->
        性别：
        <input type="radio" name="gender" value="male">男
        <input type="radio" name="gender" value="female" checked>女<br>
        <!-- 多选框 -->
        爱好：
        <input type="checkbox" name="hobby" value="smoke" checked>抽烟
        <input type="checkbox" name="hobby" value="drink">喝酒
        <input type="checkbox" name="hobby" value="perm" checked>烫头<br>
        其他：
        <textarea name="other" cols="23" rows="3"></textarea><br>
        籍贯：
        <select name="place">
            <option value="冀">河北</option>
            <option value="鲁">山东</option>
            <option value="晋" selected>山西</option>
            <option value="粤">广东</option>
        </select>
        <!-- 隐藏域 -->
        <input type="hidden" name="from" value="toutiao">
        <br>
        <!-- 确认按钮_第一种写法 -->
        <button type="submit">确认</button>
        <!-- 确认按钮_第二种写法 -->
        <!-- <input type="submit" value="确认"> -->
        <!-- 重置按钮_第一种写法 -->
        <!-- <button type="reset">重置</button> -->
        <!-- 重置按钮_第二种写法 -->
        <input type="reset" value="点我重置">
        <!-- 普通按钮_第一种写法 -->
        <input disabled type="button" value="检测账户是否被注册">
        <!-- 普通按钮_第二种写法 -->
        <!-- <button type="button">检测账户是否被注册</button> -->
    </form>
</body>

</html>
```

- 注意查看 "账户" 输入框和 "检测账户是否被注册" 按钮，二者使用了 disabled 属性，页面上被置灰不可用。

  ![image-20231209214817955](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093853472-1281556929.png)

#### label 标签

`label`标签可与表单控件相关联，关联之后点击文字，与之对应的表单控件就会获取焦点。

两种与 label 关联方式如下：

1. 让 label 标签的 for 属性的值，等于表单控件的 id。
2. 把表单控件套在 label 标签的里面。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表单_label标签</title>
</head>

<body>
    <form action="https://search.jd.com/search">
        <label for="zhanghu">账户：</label>
        <input id="zhanghu" type="text" name="account" maxlength="10"><br>
        <label>
            密码：
            <input id="mima" type="password" name="pwd" maxlength="6">
        </label>
        <br>
        性别：
        <input type="radio" name="gender" value="male" id="nan">
        <label for="nan">男</label>
        <label>
            <input type="radio" name="gender" value="female" id="nv">女
        </label>
        <br>
        爱好：
        <label>
            <input type="checkbox" name="hobby" value="smoke">抽烟
        </label>
        <label>
            <input type="checkbox" name="hobby" value="drink">喝酒
        </label>
        <label>
            <input type="checkbox" name="hobby" value="perm">烫头
        </label><br>
        <label for="qita">其他：</label>
        <textarea id="qita" name="other" cols="23" rows="3"></textarea><br>
        籍贯：
        <select name="place">
            <option value="冀">河北</option>
            <option value="鲁">山东</option>
            <option value="晋">山西</option>
            <option value="粤">广东</option>
        </select>
        <input type="hidden" name="from" value="toutiao">
        <br>
        <input type="submit" value="确认">
        <input type="reset" value="点我重置">
        <input type="button" value="检测账户是否被注册">
    </form>
</body>

</html>
```

#### fieldset 与 legend 的使用

`fieldset`可以为表单控件分组，`legend`标签是分组的标题。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>表单_label标签</title>
</head>

<body>
    <form action="https://search.jd.com/search">
        <label for="zhanghu">账户：</label>
        <input id="zhanghu" type="text" name="account" maxlength="10"><br>
        <label>
            密码：
            <input id="mima" type="password" name="pwd" maxlength="6">
        </label>
        <br>
        性别：
        <input type="radio" name="gender" value="male" id="nan">
        <label for="nan">男</label>
        <label>
            <input type="radio" name="gender" value="female" id="nv">女
        </label>
        <br>
        爱好：
        <label>
            <input type="checkbox" name="hobby" value="smoke">抽烟
        </label>
        <label>
            <input type="checkbox" name="hobby" value="drink">喝酒
        </label>
        <label>
            <input type="checkbox" name="hobby" value="perm">烫头
        </label><br>
        <label for="qita">其他：</label>
        <textarea id="qita" name="other" cols="23" rows="3"></textarea><br>
        籍贯：
        <select name="place">
            <option value="冀">河北</option>
            <option value="鲁">山东</option>
            <option value="晋">山西</option>
            <option value="粤">广东</option>
        </select>
        <input type="hidden" name="from" value="toutiao">
        <br>
        <input type="submit" value="确认">
        <input type="reset" value="点我重置">
        <input type="button" value="检测账户是否被注册">
    </form>
</body>

</html>
```

效果：

![image-20231209215944175](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093853144-1931471406.png)

### 框架标签

| 标签名 | 标签语义                     | 属性                                                         | 单/双标签 |
| ------ | ---------------------------- | ------------------------------------------------------------ | --------- |
| iframe | 框架（在网页中嵌入其他文件） | `name`：框架名称，可以与 target 属性配置<br />`width`：框架的宽度<br />`height`：框架的高度<br />`frameborder`：是否显示边框，值为 0 或者 1 | 双        |

>iframe 标签的实际应用：
>
>1. 在网页中嵌入广告。
>2. 与超链接或表单的 target 配合，展示不同的内容。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>框架标签</title>
</head>

<body>
    <!-- 利用iframe嵌入一个普通网页 -->
    <iframe src="https://www.toutiao.com" width="900" height="300" frameborder="0"></iframe>
    <br>

    <!-- 利用iframe嵌入一个广告网页 -->
    <!-- <iframe width="300" height="250" src="https://pos.baidu.com/xchm?conwid=300&conhei=250&rdid=5841072&dc=3&di=u5841072&s1=2619619085&s2=735691552&dri=1&dis=0&dai=24&ps=2557x1059&enu=encoding&exps=110283,110277,110273,110261,110254,110011&ant=0&psi=ec321235870ce038&dcb=___adblockplus_&dtm=HTML_POST&dvi=0.0&dci=-1&dpt=none&tpr=1675046784135&ti=%E7%BD%91%E6%98%93%E6%96%B0%E9%97%BB&ari=2&ver=0129&dbv=2&drs=4&pcs=1519x763&pss=1519x11348&cfv=0&cpl=5&chi=2&cce=true&cec=UTF-8&tlm=1675046701&prot=2&rw=763&ltu=https%3A%2F%2Fnews.163.com%2F&ecd=1&fpt=TSqlzRKQUoc9+DbwoBg+b3cB10ATMyxUj3wjiV0jemwIVBo7z2ECouhUiVZq9IXN5cuofEzg/QDLSl5smhScYpTN+HQc3+QhnKv3H8MhzCYvAEcKDXAQAxK1FTXUdEd7J70MlzGWjb5DY6rlVbwmYbud1lCLmHxH5enja3K/dBHQzpLvsZCQqnanh/vBkkBTauX5z2jCEQvudlFgU1sHGA2kmnPoF0fHQA756T+sNKjATCqWL62CuVSrPm52Es2xtwueTF6sREr61IdA4wcZwEObe67hCIHPeGk5UX48Fw06RMTjgGDr6oQhyEpAeW3u9Gi0qHTYg8wBI1yoBmmwhuh0MpxtrJcLm0tGY4ODYGriOVhYwo/vU1cGOPrxvZG39yCve9+xcc7sVW4DBkCezA==|2UoaY428DIL/VGPLaRon4l5i5WbAevIWwjj0W0sj4LU=|10|d42cad75cac5486feb0f88674f9a220a&dft=0&uc=1536x834&pis=-1x-1&sr=1536x864&tcn=1675046784&qn=75c4f389da0f062c&ft=1" frameborder="0"></iframe> -->
    <br>

    <!-- 利用iframe嵌入其他内容，例如图片（浏览器能打开的，都可以嵌入，无法打开的会提示下载，如.zip文件） -->
    <iframe src="./resource/小姐姐.gif" frameborder="0"></iframe>
    <br>

    <!-- 与超链接的target属性配合使用 -->
    <a href="https://www.toutiao.com" target="container">点我看新闻</a>
    <a href="https://www.taobao.com" target="container">点我看淘宝</a><br>
    <!-- 与表单的target属性配合使用 -->
    <form action="https://so.toutiao.com/search" target="container">
        <input type="text" name="keyword">
        <input type="submit" value="搜索">
    </form>

    <!-- 超链接或者表单的target属性，与iframe的name属性相同，配合使用以展示不同的内容 -->
    <iframe name="container" frameborder="0" width="900" height="300"></iframe>

</body>

</html>
```

### 字符实体

在 HTML 中，我们可以用一种**特殊的形式**的内容，来表示某个**符号**，这种特殊形式的内容，就是 HTML `字符实体`。比如小于号 < 是用于定义 HTML 标签的开始，如果我们希望浏览器正确地显示 > 这种字符，就必须在 HTML 源码中插入字符实体。

**字符实体**由三部分组成：一个`&`和 一个`实体名称`（或者一个`#`和 一个`实体编号`），最后加上一个分号`;`。

常见字符实体：

![image-20231209231259002](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093852678-798780274.png)

> 完整实体列表，参考：https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>HTML字符实体</title>
</head>

<body>
    <div>我 美女</div>
    <div>我&nbsp;&nbsp;&nbsp;&nbsp;美女</div>
    <div>我&#160;&#160;&#160;&#160;美女</div>
    <div>我们学习过很多的标题标签，其中&lt;h1&gt;是最厉害的一个！</div>
    <div>我们今天学习了一个可以表示空格的字符实体，它是：&amp;nbsp;</div>
    <div>我们今天学习了一个可以表示&的字符实体，它是：&amp;amp;</div>
    <div>当前商品的价格是：￥199</div>
    <div>当前商品的价格是：¥199</div>
    <div>当前商品的价格是：&yen;199</div>
    <div>©版权所有</div>
    <div>&copy;版权所有</div>
    <div>2 * 2 = 4</div>
    <div>2 x 2 = 4</div>
    <div>2 × 2 = 4</div>
    <div>2 &times; 2 = 4</div>
    <div>2 / 2 = 1</div>
    <div>2 ÷ 2 = 1</div>
    <div>2 &divide; 2 = 1</div>
</body>

</html>
```

### 全局属性

常用的全局属性：

| 属性名  | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| `id`    | 给标签指定唯一标识，注意：`id 是不能重复的`<br />作用：可以让 label 标签与表单控件相关联；也可以与 CSS 、 JavaScript 配合使用<br />不能在以下 HTML 元素中使用：`<head>`、`<html>`、`<meta>`、`<script>`、`<style>`、`<title>` |
| `class` | 给标签指定类名，随后通过 CSS 就可以给标签设置样式            |
| `style` | 给标签设置 CSS 样式                                          |
| `dir`   | 内容的方向，值：ltr 、 rtl<br />不能在以下 HTML 元素中使用：`<head>`、`<html>`、`<meta>`、`<script>`、`<style>`、`<title>` |
| `title` | 给标签设置一个文字提示，一般超链接和图片用得比较多。         |
| `lang`  | 给标签指定语言<br />不能在以下 HTML 元素中使用：`<head>`、`<html>`、`<meta>`、`<script>`、`<style>`、`<title>` |

> 完整的全局属性，参考：https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>HTML全局属性</title>
    <style>
        .student {
            color: red;
        }
    </style>
</head>

<body>
    <div id="hello1">你好啊！</div>
    <div id="hello2">你好啊2！</div>
    <div class="student">张三</div>
    <div class="student">李四</div>
    <div style="color:green">旺财</div>
    <bdo dir="rtl">你是年少的欢喜</bdo>
    <div dir="rtl">你是年少的欢喜</div>
    <div title="英雄联盟">LOL</div>
    <a href="https://www.baidu.com" title="去百度">去百度</a>
    <div lang="en">hello</div>
</body>

</html>
```

### meta 元信息

meta 元信息，也就是网页的基本信息，常见的有：

- 配置字符编码：

  ```html
  <meta charset="utf-8">
  ```

- 针对 IE 浏览器的兼容性配置：

  ```html
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  ```

- 针对移动端的配置：

  ```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  ```

- 配置网页关键字：

  ```html
  <meta name="keywords" content="8-12个以英文逗号隔开的单词/词语">
  ```

- 配置网页描述信息：

  ```html
  <meta name="description" content="80字以内的一段话，与网站内容相关">
  ```

- 针对搜索引擎爬虫配置：

  ```html
  <meta name="robots" content="此处可选值见下表">
  ```

  | 值        | 描述                             |
  | --------- | -------------------------------- |
  | index     | 允许搜索爬虫索引此页面           |
  | noindex   | 要求搜索爬虫不索引此页面         |
  | follow    | 允许搜索爬虫跟随此页面上的链接   |
  | nofollow  | 要求搜索爬虫不跟随此页面上的链接 |
  | all       | 与 index，follow 等价            |
  | none      | 与 noindex，nofollow 等价        |
  | noarchive | 要求搜索引擎不缓存页面内容       |
  | nocache   | noarchive 的替代名称             |

- 配置网页作者：

  ```html
  <meta name="author" content="tony">
  ```

- 配置网页生成工具：

  ```html
  <meta name="generator" content="Visual Studio Code">
  ```

- 配置定义网页版权信息：

  ```html
  <meta name="copyright" content="2023-2027©版权所有">
  ```

- 配置网页自动刷新：

  ```html
  <meta http-equiv="refresh" content="10;url=http://www.baidu.com">
  ```

  > content 中的参数，10 表示每隔 10 秒，url 表示跳转到的地方，如果不配做 url，则是每隔 10 秒，刷新当前页面一次。

> 完整的网页元信息，参考：https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <!-- 配置字符编码 -->
    <meta charset="UTF-8">
    <!-- 针对IE浏览器的一个兼容性设置 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- 针对移动端的一个配置 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 配置网页的关键字 -->
    <meta name="keywords" content="网上购物,电商购物,皮鞋,化妆品">
    <!-- 配置网页描述信息 -->
    <meta name="description" content="哈哈购物网成立于2003年，致力于打造国内优质的电商购物平台....">
    <!-- 自动刷新 -->
    <meta http-equiv="refresh" content="3">
    <title>meta元信息</title>
</head>

<body>
    <h1>你好啊</h1>
</body>

</html>
```

## HTML 5

HTML 5 是新一代的 HTML 标准，2014 年 10 月，由万维网联盟（ W3C ）完成标准制定。

官网地址：

- W3C 提供：https://www.w3.org/TR/html/index.html

- WHATWG 提供：https://whatwg-cn.github.io/html/multipage
- 二者之间存在一些差异，开发时一般选择二者都有的标签。

> HTML 5 在狭义上是指新一代的 HTML 标准，在广义上是指整个前端。

**优势：**

- 针对 JavaScript，新增了很多可操作的接口。
- 新增了一些语义化标签、全局属性。
- 新增了多媒体标签，可以很好的替代 flash。
- 更加侧重语义化，对于 SEO 更友好。
- 可移植性好，可以大量应用在移动设备上。

**兼容性：**

- 支持：Chrome、Safari、Opera、Firefox 等主流浏览器。
- IE 浏览器必须是 9 及以上版本才支持 HTML 5，且 IE 9 仅支持部分 HTML 5 新特性。

### 新增语义化标签

#### 新增布局标签

| 标签名    | 语义                                                         | 单/双标签 |
| --------- | ------------------------------------------------------------ | --------- |
| `header`  | 整个页面，或部分区域的头部                                   | 双        |
| `footer`  | 整个页面，或部分区域的底部                                   | 双        |
| `nav`     | 导航                                                         | 双        |
| `article` | 文章、帖子、杂志、新闻、博客、评论等                         | 双        |
| `section` | 页面中的某段文字，或文章中的某段文字（里面文字通常里面会包含标题） | 双        |
| `aside`   | 侧边栏                                                       | 双        |
| main      | 文档的主要内容（WHATWG 中没有语义，IE 不支持），几乎不用     | 双        |
| hgroup    | 包裹连续的标题，如文章主标题、副标题的组合 （W3C 将其删除），几乎不用 | 双        |

> 关于 article 和 section：
>
> 1. article 里面可以有多个 section。
> 2. section 强调的是分段或分块，如果你想将一块内容分成几段的时候，可使用 section 元素。
> 3. article 比 section 更强调独立性，一块内容如果比较独立、比较完整，应该使用 article 元素。

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>01_新增布局标签</title>
</head>

<body>
    <!-- 头部 -->
    <header class="page-header">
        <h1>尚品汇</h1>
    </header>
    <hr>
    <!-- 主导航 -->
    <nav>
        <a href="#">首页</a>
        <a href="#">订单</a>
        <a href="#">购物车</a>
        <a href="#">我的</a>
    </nav>
    <!-- 主要内容 -->
    <div class="page-content">
        <!-- 侧边栏导航 -->
        <aside style="float: right;">
            <nav>
                <ul>
                    <li><a href="#">秒杀专区</a></li>
                    <li><a href="#">会员专区</a></li>
                    <li><a href="#">领取优惠券</a></li>
                    <li><a href="#">品牌专区</a></li>
                </ul>
            </nav>
        </aside>
        <!-- 文章 -->
        <article>
            <h2>《如何一夜暴富》</h2>
            <section>
                <h3>第一种方式：通过做梦</h3>
                <p>你要这么做梦：xxxxxxxxxxxxxxxxxxxxxxx</p>
            </section>
            <section>
                <h3>第二种方式：通过买彩票</h3>
                <p>你要这么买彩票：xxxxxxxxxxxxxxxxxxxxxxx</p>
            </section>
            <section>
                <h3>第三种方式：通过学习前端</h3>
                <p>你要这么学前端：xxxxxxxxxxxxxxxxxxxxxxx</p>
            </section>
        </article>
    </div>
    <hr>
    <footer>
        <nav>
            <a href="#">友情链接1</a>
            <a href="#">友情链接2</a>
            <a href="#">友情链接3</a>
            <a href="#">友情链接4</a>
        </nav>
    </footer>
</body>

</html>
```

#### 新增状态标签

##### meter 标签

**语义：定义已知范围内的标量测量，也被称为 gauge（尺度），双标签。**例如：电量、磁盘用量等。

**常用属性：**

| 属性名    | 值   | 描述       |
| --------- | ---- | ---------- |
| `high`    | 数值 | 规定高值   |
| `low`     | 数值 | 规定低值   |
| `max`     | 数值 | 规定最大值 |
| `min`     | 数值 | 规定最小值 |
| `optimum` | 数值 | 规定最优值 |
| `value`   | 数值 | 规定当前值 |

- 加上 optimum 和不加 optimum，是不同的效果。

##### progress 标签

**语义：显示某个任务完成的进度的指示器，一般用于表示进度条，双标签。**例如：工作完成进度等。

**常用属性：**

| 属性名  | 值   | 描述       |
| ------- | ---- | ---------- |
| `max`   | 数值 | 规定目标值 |
| `value` | 数值 | 规定当前值 |

##### 示例

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>02_新增状态标签</title>
    <style>
        progress {
            width: 50px;
        }
    </style>
</head>

<body>
    <span>手机电量：</span>
    <!-- 增加和删除optimum标签，再变化value值，查看效果变化 -->
    <meter max="100" min="0" value="90" low="10" high="20" optimum="90"></meter>
    <br>
    <span>当前进度：</span>
    <progress max="100" value="20"></progress>
</body>

</html>
```

效果图：

![image-20231225222440699](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240917093852073-395613807.png)

#### 新增列表标签

| 标签名     | 语义                                        | 单/双标签 |
| ---------- | ------------------------------------------- | --------- |
| `datalist` | 用于搜索框的关键字提示                      | 双        |
| `details`  | 用于展示问题和答案，或对专有名词进行解释    | 双        |
| `summary`  | 写在 details 的里面，用于指定问题或专有名词 | 双        |

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>03_新增列表标签</title>
</head>

<body>
    <form action="#">
        <input type="text" list="mydata">
        <button>搜索</button>
    </form>
    <datalist id="mydata">
        <option value="周杰伦">周杰伦</option>
        <option value="周冬雨">周冬雨</option>
        <option value="马冬梅">马冬梅</option>
        <option value="温兆伦">温兆伦</option>
    </datalist>
    <hr>
    <details>
        <summary>如何一夜暴富？</summary>
        <p>来学习前端</p>
    </details>
</body>

</html>
```

#### 新增文本标签

##### 文本注音

| 标签名 | 语义                            | 单/双标签 |
| ------ | ------------------------------- | --------- |
| `ruby` | 包裹需要注音的文字              | 双        |
| `rt`   | 写注音，rt 标签写在 ruby 的里面 | 双        |

##### 文本标记

| 标签名 | 语义 | 单/双标签 |
| ------ | ---- | --------- |
| `mark` | 标记 | 双        |

- 注意：W3C 建议 mark 用于标记搜索结果中的关键字。

##### 示例

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>04_新增文本标签</title>
</head>

<body>
    <ruby>
        <span>魑魅魍魉</span>
        <rt>chī mèi wǎng liǎng </rt>
    </ruby>
    <hr>
    <div>
        <ruby>
            <span>魑</span>
            <rt>chī</rt>
        </ruby>
        <ruby>
            <span>魅</span>
            <rt>mèi</rt>
        </ruby>
    </div>
    <hr>
    <p>Lorem ipsum <mark>dolor</mark> sit amet consectetur adipisicing elit. Laboriosam, nemo?</p>
</body>

</html>
```

### 新增表单功能

#### 表单控件新增属性

| 属性名         | 功能                                                         |
| -------------- | ------------------------------------------------------------ |
| `placeholder`  | 提示文字，适用于**文字输入类**的表单控件<br />注意：不是默认值，默认值是 value |
| `required`     | 表示该输入项必填， 适用于**除按钮外**其他表单控件            |
| `autofocus`    | 自动获取焦点，适用于所有表单控件                             |
| `autocomplete` | 自动完成，可以设置为 on 或 off，适用于**文字输入类**的表单控件<br />注意：密码输入框、多行输入框不可用 |
| `pattern`      | 填写正则表达式，适用于文本输入类表单控件<br />注意：多行输入不可用，且空的输入框不会验证，往往与 required 配合 |

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>01_新增的表单控件属性</title>
</head>

<body>
    <form action="">
        账号：<input type="text" name="account" placeholder="请输入账号" required autofocus autocomplete="on" pattern="\w{6}">
        <br>
        密码：<input type="password" name="pwd" placeholder="请输入密码" required pattern="\w{6}">
        <br>
        性别：
        <input type="radio" value="male" name="gender" required>男
        <input type="radio" value="female" name="gender">女
        <br>
        爱好：
        <input type="checkbox" value="smoke" name="hobby">抽烟
        <input type="checkbox" value="drink" name="hobby" required>喝酒
        <input type="checkbox" value="perm" name="hobby">烫头
        <br>
        其他：<textarea name="other" placeholder="请输入密码" required pattern="\w{6}"></textarea>
        <br>
        <button>提交</button>
    </form>
</body>

</html>
```

####  input 新增属性值

| 属性名           | 功能                                                         |
| ---------------- | ------------------------------------------------------------ |
| `email`          | **邮箱**类型的输入框，表单提交时会验证格式，输入为空则不验证格式 |
| `url`            | **url** 类型的输入框，表单提交时会验证格式，输入为空则不验证格式 |
| `number`         | **数字**类型的输入框，表单提交时会验证格式，输入为空则不验证格式 |
| `search`         | **搜索**类型的输入框，表单提交时不会验证格式                 |
| `tel`            | **电话**类型的输入框，表单提交时不会验证格式，在移动端使用时，会唤起数字键盘 |
| `range`          | **范围**选择框，默认值为 50，表单提交时不会验证格式          |
| `color`          | **颜色**选择框，默认值为黑色，表单提交时不会验证格式         |
| `date`           | **日期**选择框，默认值为空，表单提交时不会验证格式           |
| `month`          | **月份**选择框，默认值为空，表单提交时不会验证格式           |
| `week`           | **周**选择框，默认值为空，表单提交时不会验证格式             |
| `time`           | **时间**选择框，默认值为空，表单提交时不会验证格式           |
| `datetime-local` | **日期 + 时间**选择框，默认值为空，表单提交时不会验证格式    |

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>02_input新增的type属性值</title>
</head>

<body>
    <form action="" novalidate>
        邮箱：<input type="email" required name="email">

        <!-- url：<input type="url" required name="url"> -->

        <!-- 数值：<input type="number" required name="number" step="2" max="80" min="20"> -->

        <!-- 搜索：<input type="search" required name="keyword"> -->

        <!-- 电话：<input type="tel" required name="phone"> -->

        <!-- 光照强度：<input type="range" name="range" max="80" min="20" value="22"> -->

        <!-- 颜色：<input type="color" name="color"> -->

        <!-- 日期：<input type="date" required name="date"> -->

        <!-- 月份：<input type="month" required name="month"> -->

        <!-- 周：<input type="week" required name="week"> -->

        <!-- 时间：<input type="time" required name="time"> -->

        <!-- 日期+时间：<input type="datetime-local" required name="time2"> -->

        <br>
        <button>提交</button>
    </form>
</body>

</html>
```

#### form 标签新增属性

| 属性名       | 功能                                                     |
| ------------ | -------------------------------------------------------- |
| `novalidate` | 如果给 form 标签设置了该属性，表单提交的时候不再进行验证 |

### 新增多媒体标签

#### 视频标签

`<video>`：用来定义视频，双标签。

| 属性名     | 值                       | 描述                                                         |
| ---------- | ------------------------ | ------------------------------------------------------------ |
| `src`      | URL 地址                 | 视频地址                                                     |
| `width`    | 像素值                   | 设置视频播放器的宽度                                         |
| `height`   | 像素值                   | 设置视频播放器的高度                                         |
| `controls` | -                        | 向用户显示视频控件（比如播放/暂停按钮）                      |
| `muted`    | -                        | 视频静音                                                     |
| `autoplay` | -                        | 视频自动播放                                                 |
| `loop`     | -                        | 循环播放                                                     |
| `poster`   | URL 地址                 | 视频封面                                                     |
| `preload`  | `auto / metadata / none` | 视频预加载，如果使用 autoplay，则忽略该属性<br />        auto：可以下载整个视频文件，即使用户不希望使用它<br />        metadata：仅预先获取视频的元数据（例如长度）<br />        none：不预加载视频 |

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>01_新增视频标签</title>
    <style>
        video {
            width: 600px;
        }
    </style>
</head>

<body>
    <video src="./小电影.mp4" controls muted loop poster="./封面.png" preload="auto"></video>
</body>

</html>
```

#### 音频标签

`<audio>`：用来定义音频，双标签。

| 属性名     | 值                       | 描述                                                         |
| ---------- | ------------------------ | ------------------------------------------------------------ |
| `src`      | URL 地址                 | 音频地址                                                     |
| `controls` | -                        | 向用户显示音频控件（比如播放/暂停按钮）                      |
| `autoplay` | -                        | 音频自动播放                                                 |
| `muted`    | -                        | 音频静音                                                     |
| `loop`     | -                        | 循环播放                                                     |
| `preload`  | `auto / metadata / none` | 音频预加载，如果使用 autoplay，则忽略该属性<br />        auto：可以下载整个音频文件，即使用户不希望使用它<br />        metadata：仅预先获取音频的元数据（例如长度）<br />        none：不预加载音频 |

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>02_新增音频标签</title>
    <style>
        audio {
            width: 600px;
            /* height: 500px; */
            border: 1px solid black;
        }
    </style>
</head>

<body>
    <audio src="./小曲.mp3" controls loop preload="metadata"></audio>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>03_音频案例</title>
    <style>
        audio {
            width: 600px;
        }

        .mask {
            position: fixed;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            background-color: rgba(0, 0, 0, 0.727);
        }

        .dialog {
            position: absolute;
            width: 400px;
            height: 400px;
            background-color: green;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            margin: auto;
            font-size: 40px;
            text-align: center;
            line-height: 400px;
        }

        span {
            border: 1px solid white;
            color: white;
            cursor: pointer;
        }
    </style>
</head>

<body>
    <audio id="music" src="./小曲.mp3" controls></audio>
    <div class="mask" id="mask">
        <div class="dialog">
            <span>点我登录</span>
            <span onclick="go()">随便听听</span>
        </div>
    </div>

    <script>
        function go() {
            music.play()
            mask.style.display = 'none'
        }
    </script>
</body>

</html>
```

### 新增全局属性（了解）

| 属性名            | 功能                                                         |
| ----------------- | ------------------------------------------------------------ |
| `contenteditable` | 表示元素是否可被用户编辑，可选值如下：<br />        true：可编辑<br />        false：不可编辑 |
| `draggable`       | 表示元素可以被拖动，可选值如下：<br />        true：可拖动<br />        false：不可拖动 |
| `hidden`          | 隐藏元素                                                     |
| `spellcheck`      | 规定是否对元素进行拼写和语法检查，可选值如下：<br />        true：检查<br />        false：不检查 |
| `contextmenu`     | 规定元素的上下文菜单，在用户鼠标右键点击元素时显示           |
| `data-*`          | 用于存储页面的私有定制数据                                   |

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>新增的全局属性</title>
    <style>
        div {
            width: 600px;
            height: 200px;
            border: 1px solid black;
            font-size: 20px;
            margin-bottom: 10px;
        }

        .box1 {
            background-color: skyblue;
        }

        .box2 {
            background-color: green;
        }
    </style>
</head>

<body>
    <div class="box1" spellcheck="true" contenteditable="true">Lorem ipsum dolor sit amet.</div>
    <div class="box2" draggable="true" hidden data-a="1" data-b="2" data-c="3">Lorem ipsum dolor sit amet.</div>
    <h1>hello</h1>

</body>

</html>
```

### HTML 5 兼容性处理

**方式一：添加元信息，让浏览器处于最优渲染模式。**

```html
<!--设置IE总是使用最新的文档模式进行渲染-->
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
<!--优先使用 webkit ( Chromium ) 内核进行渲染, 针对360等壳浏览器-->
<meta name="renderer" content="webkit">
```

**方式二：使用`html5shiv`让低版本浏览器认识 HTML 5 的语义化标签。**

```html
<!--[if lt ie 9]>
<script src="../sources/js/html5shiv.js"></script>
<![endif]-->
```

> 扩展：
>
> - `lt`：小于。
> - `lte`：小于等于。
> - `gt`：大于。
> - `gte`：大于等于。
> - `!`：逻辑非。
>
> 用法：
>
> ```html
> <!--[if IE 8]>仅IE8可见<![endif]-->
> 
> <!--[if gt IE 8]>仅IE8以上可见<![endif]—>
> 
> <!--[if lt IE 8]>仅IE8以下可见<![endif]—>
> 
> <!--[if gte IE 8]>IE8及以上可见<![endif]—>
> 
> <!--[if lte IE 8]>IE8及以下可见<![endif]—>
> 
> <!--[if !IE 8]>非IE8的IE可见<![endif]-->
> ```

示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <title>兼容性处理</title>
    <!-- 让IE浏览器处于一个最优的渲染模式 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- 针对一些国产的“双核”浏览器的设置，让浏览器优先使用webkit内核去渲染网页 -->
    <meta name="render" content="webkit">
    <!--[if lt ie 9]>
    <script src="./html5shiv.js"></script>
    <![endif]-->
    <style>
        header {
            background-color: orange;
        }

        footer {
            height: 100px;
            line-height: 100px;
            text-align: center;
            background-color: green;
        }
    </style>
</head>

<body>
    <!-- 头部 -->
    <header class="page-header">
        <h1>尚品汇</h1>
    </header>
    <hr>
    <!-- 主导航 -->
    <nav>
        <a href="#">首页</a>
        <a href="#">订单</a>
        <a href="#">购物车</a>
        <a href="#">我的</a>
    </nav>
    <!-- 主要内容 -->
    <div class="page-content">
        <!-- 侧边栏导航 -->
        <aside style="float: right;">
            <nav>
                <ul>
                    <li><a href="#">秒杀专区</a></li>
                    <li><a href="#">会员专区</a></li>
                    <li><a href="#">领取优惠券</a></li>
                    <li><a href="#">品牌专区</a></li>
                </ul>
            </nav>
        </aside>
        <!-- 文章 -->
        <article>
            <h2>《如何一夜暴富》</h2>
            <section>
                <h3>第一种方式：通过做梦</h3>
                <p>你要这么做梦：xxxxxxxxxxxxxxxxxxxxxxx</p>
            </section>
            <section>
                <h3>第二种方式：通过买彩票</h3>
                <p>你要这么买彩票：xxxxxxxxxxxxxxxxxxxxxxx</p>
            </section>
            <section>
                <h3>第三种方式：通过学习前端</h3>
                <p>你要这么学前端：xxxxxxxxxxxxxxxxxxxxxxx</p>
            </section>
        </article>
    </div>
    <hr>
    <!-- 页脚 -->
    <footer>
        <nav>
            <a href="#">友情链接1</a>
            <a href="#">友情链接2</a>
            <a href="#">友情链接3</a>
            <a href="#">友情链接4</a>
        </nav>
    </footer>
</body>

</html>
```

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/FrontEnd/html-css.md