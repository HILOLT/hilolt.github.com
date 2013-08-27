# 个人博客

[hilolt](http://hilolt.github.io)

个人博客，转载请注明出处，保留所有权利�?

## 使用的工�?

1. 图标�?        [Font-Awesome](http://fortawesome.github.io/Font-Awesome)

1. 静态页面服�?  [Github pages](http://pages.github.com )

1. 博客生成工具   [Jekyll](https://github.com/mojombo/jekyll )

1. 博客生成工具   [Jekyll-Bootstrap](http://jekyllbootstrap.com/ )

1. 前端工具�?    [JQuery](http://jquery.com/ )

1. 表格控件       [DataTables](http://www.datatables.net/ )

1. 前端框架       [Twitter Bootstrap](http://twitter.github.io/bootstrap )

1. 前端排版样式�?[Typo.css](http://typo.sofish.de )

1. 开发工�?      [Vim](http://www.vim.org/ )

## 注意

### 归档页面链接图标

归档页面文章前的符号是font-awesome的图表名称，请在post中的yaml指定icon属�?

比如想展示class名字为icon-github的图标，指定icon属性值为github即可,具体请参考post中的写法

如果使用rake文件生成post，post默认的图标是file-alt

### 导航�?

为了自定义导航栏子栏目的顺序，重写了这部分的逻辑，具体在文件_include/hilolt/navigation_list�?

为了在后台实现高亮当前的导航页，用一种不太好的方式实现了这个功能，建议使用js在页面加载后进行设置

用我自己使用的导航栏来作为示例，我的导航栏有四个，对应文件在根目录下，均为html文件

1. 文章 /posts.html

1. 时间�?/timeline.html

1. 目录 /categories.html

1. 关于 /about.html

首先color_hack这个变量存放导航栏的文件名， `{% assign color_hack = 'posts timeline categories about' %}`

比如点击导航栏的‘文章’链接，page.url变量的值是/posts.html，将这个字符串去除�?’，去除�?html’，得到字符串posts，存入current_nav变量

然后将color_hack中的current_nav替换�?active'，即将posts替换为active，得到color_hack的指�?active timeline categories about'

然后将数组分割，得到一个数组[active,timeline,categories,about]，赋值给color_hack

然后按顺序指定导航栏的html内容，顺序应与color_hack初始值中的顺序相对应，class属性的指即从对应的color_hack中按顺序获取

1. `<li class="{{color_hack[0]}}"><a href="/posts.html">文章</a></li>`

1. `<li class="{{color_hack[1]}}"><a href="/timeline.html">归档</a></li>`

1. `<li class="{{color_hack[2]}}"><a href="/categories.html">目录</a></li>`

1. `<li class="{{color_hack[3]}}"><a href="/about.html">关于</a></li>`

这样当前访问的导航的class会被指定为active，其他导航的class会被指定为各自的文件�?
