# HTML

## [HTML 基础](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Getting_started)

### 块级元素和内联元素

在HTML中有两种你需要知道的重要元素类别，块级元素和内联元素。

* 块级元素在页面中以块的形式展现 —— 相对于其前面的内容它会出现在新的一行，其后的内容也会被挤到下一行展现。块级元素通常用于展示页面上结构化的内容，例如段落、列表、导航菜单、页脚等等。一个以block形式展现的块级元素不会被嵌套进内联元素中，但可以嵌套在其它块级元素中。
* 内联元素通常出现在块级元素中并环绕文档内容的一小部分，而不是一整个段落或者一组内容。内联元素不会导致文本换行：它通常出现在一堆文字之间例如超链接元素 \<a> 或者强调元素 \<em> 和 \<strong>.
* 无论你在HTML元素的内容中使用多少空格(包括空白字符，包括换行)，当渲染这些代码的时候，HTML解释器会将连续出现的空白字符减少为一个单独的空格符。
* 我们必须使用字符引用 —— 表示字符的特殊编码, 它们可以在那些情况下使用。 每个字符引用以符号 & 开始, 以分号 ; 结束。
* 注：全球色盲患者比例为 4%，换句话说，每 12 名男性就有一位色盲，每 200 名女性就有一位色盲。全盲和视障人士约占世界人口（约 70 亿）的 13％（2015年 全球约有 9.4 亿人口存在视力问题）。
* 警告：\<div\> 非常便利但容易被滥用。由于它们没有语义值，会使 HTML 代码变得混乱。要小心使用，只有在没有更好的语义方案时才选择它，而且要尽可能少用， 否则文档的升级和维护工作会非常困难。
* 调试其实没有那么可怕，写代码和调试的关键其实是：熟悉语言本身和相关工具。
* HTML 之所以以宽松的方式进行解析，是因为 Web 创建的初心就是：人人可发布内容，不去纠结代码语法。如果 Web 以严格的风格起步，也许就不会像今天这样流行了。
* HTML 验证: 最好的方法就是让你的HTML页面通过 [Markup Validation Service](https://validator.w3.org/), 由 W3C 创立并维护的标记验证服务。把一个 HTML 文档加载至本网页并运行 ，网页会返回一个错误报告。
* 注：属性缺少结束引号会导致元素无法闭合。因为文档所有剩余部分（直到文档某处出现一个引号）都将被解析为属性的内容。
* Google推荐将图片存储在和 HTML 页面同路径的 images 文件夹下。搜索引擎也读取图像的文件名并把它们计入SEO。因此你应该给你的图片取一个描述性的文件名：dinosaur.jpg 比 img835.png 要好。
* 像 \<img\> 和 \<video\> 这样的元素有时被称之为替换元素，因为这样的元素的内容和尺寸由外部资源（像是一个图片或视频文件）所定义，而不是元素自身。
* 某些用户仍在使用纯文本的浏览器，例如 [Lynx](https://en.wikipedia.org/wiki/Lynx_%28web_browser%29), 这些浏览器会把图片替换为描述文本。
* autoplay 这个属性会使音频和视频内容立即播放，即使页面的其他部分还没有加载完全。建议不要应用这个属性在你的网站上，因为用户们会比较反感自动播放的媒体文件。
* 注意：为了提高速度，在主内容完成加载后，使用JavaScript设置iframe的src属性是个好主意。这使您的页面可以更快地被使用，并减少您的官方页面加载时间（重要的SEO指标）。
* 注意：插件是一种对浏览器原生无法读取的内容提供访问权限的软件。
* 在网上，你会和两种类型的图片打交道 — 位图和矢量图:
  * 位图使用像素网格来定义 — 一个位图文件精确得包含了每个像素的位置和它的色彩信息。流行的位图格式包括 Bitmap (.bmp), PNG (.png), JPEG (.jpg), and GIF (.gif.)
  * 矢量图使用算法来定义 — 一个矢量图文件包含了图形和路径的定义，电脑可以根据这些定义计算出当它们在屏幕上渲染时应该呈现的样子。 SVG 格式可以让我们创造用于 Web 的精彩的矢量图形。
* 当web第一次出现时，这样的问题并不存在，在上世纪90年代中期，仅仅可以通过笔记本电脑和台式机来浏览web页面，所以浏览器开发者和规范制定者甚至没有想到要实现这种解决方式（响应式开发）。最近应用的响应式图像技术，通过让浏览器提供多个图像文件来解决上述问题，比如使用相同显示效果的图片但包含多个不同的分辨率（分辨率切换），或者使用不同的图片以适应不同的空间分配（美术设计）。
* 