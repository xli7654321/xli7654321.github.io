---
layout: post
title: Jekyll & Markdown
date: 2024-05-25 12:00
description: Jekyll & Markdown Tutorial
tags: Jekyll Markdown
categories: sample-posts
---

## Jekyll & Markdown

标题：

```
# 一级标题 到 ###### 六级标题
```

强调：

```
*text* or _text_ 斜体
**text** or __text__ 加粗
***text*** or ___text___ 加粗且斜体
~~删除线~~
```

列表

```markdown
无序：
* 项目1
* 项目2
或者
- 项目3
+ 项目4

有序：
1. 项目1
2. 项目2

Task List:
- [ ] todo item
- [x] done item

定义列表（definition list）
<dl> 创建定义列表
<dt></dt> 表示要定义的项
<dd></dd> 表示该项的定义
</dl>

列表中间有插入（但列表顺序不间断）
1.  Item one
1.  Item two

Some text

{:style="counter-reset:none"}
1.  Item three
1.  Item four

指定有序列表从某一个值开始
{:style="counter-reset:step-counter 41"}
1.  Item 42
1.  Item 43
1.  Item 44

嵌套列表
- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
```

图片：

```
![图片的替代文本]（图片的url）
![](../../assets/images/small-image.jpg)
图片放在assets/images里面
```

表格：

```
|  表头  |  表头  |  表头  |
|  :---  |  :---: |  ---:  |
|  左对齐 |  居中  |  右对齐 |
|  内容   |  内容  |  内容  |
```

Jekyll会自动为md生成HTML元素

在 Kramdown（Jekyll 默认的 Markdown 解析器）中，`{:.class}`用于给某个元素添加类

`{:toc}`代表在当前位置插入一个自动生成的目录（table of contents），列出所有的标题

```
1. TOC
{:toc}
# 1. TOC == 1.代表创建一个有序列表，列表内容为TOC，TOC相当于占位符，会被{:toc}的内容替代
```

在符号（例如_）前添加 \ 表示转义，将标记符号转义为普通文本

创建内联链接

{% raw %}
```markdown
[text](text_url)
text_url的形式:
https://www.openai.com
{% link docs/posts/2023-7-14-test.md %}#heading

或者在文本之后定义
[text]

[text]: url

或者使用<>直接给某个链接标记超链接
<url>

使用span标签
<span id='xxx'></span>
[xxx](#xxx)
```
{% endraw %}

三个反引号创建一个代码块，在第一组反引号后面写上语言名称，可以语法高亮

````
```python
print("Hello, world!")
```
````

通常在页脚处定义脚注的内容`[^footnote]: content`,然后可以在其他位置进行引用`[^footnote]`（类似参考文献）,html渲染后会变成上标，可以跳转，写两个脚注时中间应空一行。

在文本之前加上`>`符号，表示这段文本为引用

```
> 这行文字是一个引用
```

Jekyll中`{{ site.title }}`的title变量来自于_config.yml中的所配置的title

在Jekyll的md中代码块中输入liquid语法，要加raw块，让Jekyll不处理

````
{% raw %}
```liquid
content
```
{% endraw %}
````

Markdowm支持在内容中直接使用HTML标签

创建一个包含目录的可折叠面板

```
<details open markdown="block"> # <details> 标签用来创建一个可折叠的内容面板, open 表示面板默认处于展开状态, markdown="block"使得 Jekyll 在 <details> 标签内解析 Markdown 语法
<summary> # <summary> 标签定义了面板的标题, 此处标题为Table of contents
Table of contents
</summary>
{: .text-delta} # 为标题添加类
- TOC # 创建无序列表，为标题自动生成目录
{:toc}
</details>

{::options toc_levels="2..4" /}
# {::options ... ，}的形式叫做 "block IAL"（Inline Attribute List），用来为整个块（block）设置属性或选项，最后的 / 表示这个 block IAL 的结束。
# toc_levels="2..4" 是一个选项表示自动生成的目录只包含二到四级标题
```

shift+Tab自动对齐

shift+Enter 换一行

Ctrl+/ 切换到markdown源代码

在 HTML 中，Tab 字符的默认宽度通常是 8 个空格。在base.scss中修改code的tab-size为4，即可使typora编辑的代码块的tab正常显示为4个空格



## Just the Docs

在docs创建文章

新建文件夹posts，在文件夹下首先posts.md，在开头设置

```
---
layout: default # 指_layout中的default.html
title: Posts
nav_order: 1
has_children: true # 可以自动生成toc
permalink: /docs/ui-components # 自定义URL
---
```

然后目录里的子内容如post1.md

```
---
layout: default
title: Post1
parent: Posts
nav_order: 1
---

<div class="code-example" markdown="1"> 
markdown="1"可以使div中的内容使用markdown格式渲染
如{: .btn}
```

### configuration

```
{: .class }
{: .no_toc }: 不生成目录（低于二级标题时可以考虑加）
.fs-6 .fw-300

设置label
.d-inline-block: 元素显示为行内块级元素
.label
.label-green/blue/purple/red/yellow

callouts:(配置文件中修改)
.highlight - yellow
.important - blue
.new - green
.note - purple
.warning - red

自定义callouts的title
{: .highlight/important/new/note/warning-title}
> My title
>
> My callout content

多段落callout
{: .important }
> A paragraph
>
> Another paragraph
>
> The last paragraph

callout缩进
> {: .new }
> > A paragraph
> >
> > Another paragraph
> >
> > The last paragraph

嵌套callout 默认背景透明
{: .important }
> {: .warning }
> A paragraph
若背景不透明，加一个div并给这个div添加opaque类
{: .important }
> {: .opaque }
> <div markdown="block">
> {: .warning }
> A paragraph
> </div>
```

Dark/Light切换btn：

```
<button class="btn js-toggle-dark-mode">Preview dark color scheme</button>

<script>
const toggleDarkMode = document.querySelector('.js-toggle-dark-mode');

jtd.addEvent(toggleDarkMode, 'click', function(){
  if (jtd.getTheme() === 'dark') {
    jtd.setTheme('light');
    toggleDarkMode.textContent = 'Preview dark color scheme';
  } else {
    jtd.setTheme('dark');
    toggleDarkMode.textContent = 'Return to the light side';
  }
});
</script>
```

在\_sass/custom/setup.scss中自定义sass，默认提供`grey-lt`, `grey-dk`, `blue`, `yellow`, `red`, `green`, `purple`,从000到300范围（\_sass/support/\_variables.scss）

just the docs的主scss文件位于_includes/css/just-the-docs.scss，其中导入了\_sass中的modules、support、custom/setup等，可以更改这里设置新的生效的scss路径（例如custom.scss必须有在主scss文件中`@import "./custom/custom"`）通常主scss位于assets/css文件夹中，这里使用了include

在导航中添加md，可以直接通过[pages](https://jekyllrb.com/docs/pages/)，也可以通过[Jekyll collections](https://jekyllrb.com/docs/collections/)，collections必须加`_`前缀（例如`_posts`），并需要在`_config.yml`中配置，

```
# 定义Jekyll的collections
collections:
	posts:
		output: true
		permalink: "/posts/:title/"  # :title是一种变量的表示方法
	tutorials:
		output: true
		permalink: "/tutorials/"

# 定义哪些collections在just-the-docs中使用
just_the_docs:
	collections:
    	posts:
    		name: Posts
    		nav_exclude: true or false
    		nav_fold: true or false
    		search_exclude: true or false
    	tutorials:
    		name: Tutorials
```

just the docs的collection将在上方docs之后在导航栏中新建一个板块以区分

如果md文件中设置了`has_children: true`会在页面上自动生成TABLE OF CONTENTS块

### UI Components

在Just the docs中样式类的最原始的定义都在\_sass/support.scss中

#### typography（排版：字体、字号、行距、对齐方式等）

```scss
_variable.scss
1) font-family：原设置（mono为代码块的字体）
$body-font-family: system-ui, -apple-system, blinkmacsystemfont, "Segoe UI", roboto, "Helvetica Neue", arial, sans-serif !default;
$mono-font-family: "SFMono-Regular", menlo, consolas, monospace !default;
2) font-size: $font-size-1 - $font-size-10
```

[font-download](https://fontshub.pro/)

在default.html添加

```html
<head>
<!-- 优化自定义字体加载（刷新页面会闪烁）问题（Flash of Unstyled Text），优先使用woff2 -->
  <link rel="preload" href="{{ site.baseurl }}/assets/css/fonts/ProximaSoft-Medium.woff2" as="font" type="font/woff2" crossorigin>
</head>
```

`@mixin` 混合器定义一个样式块，可以重复使用，然后使用`@include`在别的样式中使用这个样式

```scss
@mixin fs-1 {
	font-size: 16px;
}

.fs-1 {
	@include fs-1;
}

也可以为@mixin添加参数
@mixin border-radius($radius) {
	border-radius: $radius;
}

.br-10 {
	@include border-radius(10px);
}

例外一个例子
@mixin btn-color($fg, $bg) {
    color: $fg;
    background-color: darken($bg, 2%);
}

// & 用于表示父级选择器（parent selector）的引用
&:hover {
    color: $fg;
    background-color: darken($bg, 4%);
}
```

#### 字体

```scss
utilities/_typography.scss
font-size: .fs-1 - .fs-10
font-weight: .fw-300 - 800
// line-height: .lh-0, .lh-default: 1.4, .lh-tight: 1.25, $content-line-height: 1.6
letter-spacing: .ls-5, .ls-10, .ls-0
大写：.text-uppercase 

typography.scss
h1, .text-alpha {
    @include fs-8
}

h2/.text-beta - fs-6
h3/.text-gamma - fs-5
h4/.text-delta - fs-4
h5/.text-epsilon - fs-3
h6/.text-zeta/.text-small - fs-2
.text-mono(代码块样式)
对齐：.text-left, .text-center, .text-right(text-align属性)
```

### Buttons

```
.btn
[Link button](http://example.com/){: .btn }
.btn-purple / 必须和.btn一起用因为只是改变了颜色{: .btn .btn-purple }
.btn-blue/green/outline

使用内联HTNL元素
<button type="button" name="button" class="btn">Button element</button>

更改button字体的大小
<span class="fs-6">
[Big ass button](http://example.com/){: .btn }
</span>

增加同一行两个btn之间的间距
{: .btn .btn-purple .mr-2 }
```

### Labels

```
{: .label} default is blue
{: .label .label-blue/green/purple/yellow/red}
```

#### Mermaid

[Mermaid](https://mermaid.js.org/)用于创建流程图

````
_config.yml

mermaid:
	# Pick an available version from https://cdn.jsdelivr.net/npm/mermaid/
	# 指定版本
	version: "9.1.6"
# 然后在_includes/mermaid_config.js进行进一步的设置
默认为空
{}

markdown中例子：
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```
````

#### Code

```markdown
{% highlight yaml %}
kramdown:
  syntax_highlighter_opts:
    block:
      line_numbers: true
{% endhighlight %}

在md中使用{% highlight yaml %}{% endhighlight %}用于指定代码块中的代码类型并且不用再添加反引号``` ```
```



### Search

search功能通过lunr.js实现，位置assets/js/vendor/lunr.min.js(min.js相比于.js去除了注释等，减小了文件的大小，以便在生产环境中使用，.js是完整源代码)，以`page title`,`page content`,`page url`进行索引

防止page被搜索，在md的开头添加`search_exclude: true`,在nav中隐藏`nav_exclude: true`

#### 展示代码样例

````markdown
<div class="code-example" markdown="1">
text
</div>

```python
code
```
````

### Utilities

#### Colors

```
_sass/support/_variables.scss
_sass/utilities/_colors.scss

light grey / dark grey / purple / blue / green / yellow / red
000 - 300
font color: .text-purple-000
background color: .bg-purple-000
```

#### Layout

```
Spacing 间距

spacing的值根据 1rem = 16px
1 0.25rem
2 0.5rem
3 0.75rem
4 1rem
5 1.5rem
6 2rem
7 2.5rem
8 3rem
auto

margin:
// .m-0, .m-1, .m-2
.m-
.mx- x轴方向
.my-
.mt- margin-top
.mr-
.mb-
.ml-

padding:
.p-
.px- x轴方向
.py-
.pt- padding-top
.pr-
.pb-
.pl-

水平对齐
.flex-justify-start justify-content: flex-start
.flex-justify-end
.flex-justify-between
.flex-justify-around

垂直对齐
.v-align-baseline vertical-align: baseline
.v-align-bottom
.v-align-middle
.v-align-text-bottom
.v-align-text-top
.v-align-top

Display
.d-block display: block
.d-flex
.d-inline
.d-inline-block
.d-none
```

#### 响应式

```
上述定义的layout类均可使用响应式类
例如 .d-sm-block

xs 	320px (20rem) and up
sm 	500px (31.25rem) and up
md 	740px (46.25rem) and up
lg 	1120px (70rem) and up
xl 	1400px (87.5rem) and up
```

两种布局

```
layout: default

layout: minimal
省略了侧边栏和导航，只有单纯的内容页面
可以用于一些特殊的跳转

点明parent, grand_parent
如果有has_children, 会自动生成TOC
```

返回主页面语法

```markdown
[Return to main website]({{site.baseurl}}/).
```

外部链接在配置中设置

```yaml
# External navigation links
# 导航栏位于最后的外部链接（带图标）
nav_external_links:
  - title: Uni-CYPred
    url: http://47.115.205.248:8080
```

#### Navigation Structure 导航结构

```
定义Page的顺序, 在开头设置 nav_order
---
layout: default
title: Test
nav_order: 2
---

配置文件中设置
nav_sort: case_sensitive # 表示大写在小写前面

不希望出现在导航中的Page，同时没有title的页面也会被排除
---
layout: default
title: 404
nav_exclude: true
---

有子页面的Page(父页面)
has_children: true

子页面（目前）：
parent: xxx
grand_parent: xxx
nav_order: 2

反转子页面默认的排序顺序
child_nav_order: reversed

有子页面的page会自动生成TOC
如果要禁止
has_toc: false

aux_links 辅助链接（右上角） - 配置中设置

外部链接 - 配置中设置

页面中目录
# Navigation Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

可折叠目录
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

```

### Customization

{% raw %}
```markdown
切换 color scheme
<button class="btn js-toggle-dark-mode">Preview dark color scheme</button>

<script>
const toggleDarkMode = document.querySelector('.js-toggle-dark-mode');

jtd.addEvent(toggleDarkMode, 'click', function(){
  if (jtd.getTheme() === 'dark') {
    jtd.setTheme('light');
    toggleDarkMode.textContent = 'Preview dark color scheme';
  } else {
    jtd.setTheme('dark');
    toggleDarkMode.textContent = 'Return to the light side';
  }
});
</script>

添加自定义的color scheme
添加文件 _sass/color_schemes/myscheme.scss 进行样式覆盖即可
默认以light为基础设置
如果想要以dark为基础设置，添加 @import "../color_schemes/dark";
然后在_config.yml中设置 color_scheme: myscheme
如果想通过Javascript动态改变scheme，需要添加文件
assets/css/just-the-docs-myscheme.scss
在文件中添加：
---
---
{% include css/just-the-docs.scss.liquid color_scheme="myscheme" %}
然后在js中则是：jtd.setTheme("myscheme")

添加新的变量：
// _sass/custom/setup.scss
$pink-000: #f77ef1;
$pink-100: #f967f1;
$pink-200: #e94ee1;
$pink-300: #dd2cd4;

在_includes中的一些自定义
_includes/toc_heading_custom.html
_includes/footer_custom.html
_includes/head_custom.html
_includes/header_custom.html
_includes/nav_footer_custom.html
_includes/search_placeholder_custom.html

页面的一些组成部分
_includes/head.html
_includes/icons/icons.html
_includes/components/sidebar.html
_includes/components/header.html
_includes/components/breadcrumbs.html
_includes/vendor/anchor_headings.html
_includes/components/children_nav.html
_includes/components/footer.html
_includes/components/search_footer.html
_includes/components/mermaid.html

更改default页面 _layouts/default.html
```
{% endraw %}

网站内部link格式

{% raw %}
```markdown
[Configuration]({% link docs/customization.md %}#override-includes)
```
{% endraw %}

标题写法

```markdown
## Header 2

## [](#header-2)Header 2
```

导航栏/breadcrumb栏/右上角aux link的字体样式在\_sass/navigation.scss中设置

更改default.html页面最下方到浏览器底部的距离（对class="main-content-wrap"修改，在\_sass/layout.scss）





### Jekyll完整流程

安装Ruby

[Ruby下载](https://rubyinstaller.org/downloads/)  检查安装 `ruby -v`

安装Jekyll和bundle

在cmd中执行`gem install bundler jekyll`

然后从模板的github上`git clone`, cmd进入目录下，执行`bundle install`

本地运行：`bundle exec jekyll serve`

上传github.io，仓库名必须为 username.github.io

```
git remote -v
git remote remove origin
git remote add origin git@github.com:dreamlessdrugs/dreamlessdrugs.github.io.git add
git add .
git commit -m "Initial commit"
git push -u origin main
```

支持中文搜索功能

从https://github.com/MihaiValentin/lunr-languages中下载js文件，然后在head.html 中添加

```html
<script src="{{ '/assets/js/vendor/lunr.stemmer.support.js' | relative_url }}"></script>
<script src="{{ '/assets/js/vendor/lunr.zh.js' | relative_url }}"></script>
<script src="{{ '/assets/js/vendor/lunr.multi.js' | relative_url }}"></script>
```

然后在 `asset/js/just-the-docs.js` 的 initSearch() 函数中的lunr.js 的初始化模块添加

```javascript
var index = lunr(function(){
  this.use(lunr.multiLanguage('en', 'zh')); // add
  this.ref('id');
  this.field('title', { boost: 200 });
  this.field('content', { boost: 2 });
  // ... rest of the code
});
```

**使用 `<span id="xxx"></span>` 标签创建页内链接。**
**使用 `<span class="xxx"></span>` 标签添加自定义样式。**



https://zhuanlan.zhihu.com/p/613068160

https://www.taniarascia.com/make-a-static-website-with-jekyll/

