最近在用 Python 写爬虫，爬取 豆瓣电影Top250 的相关数据，需要快速提取页面中用到的数据，因此使用了 Beautiful Soup 这个 Python 库，以下为学习笔记。

### 1. 为什么使用 Beautiful Soup？
使用 Python 将网页的数据爬取下来后，如何在爬取到的数据中快速提取出我们想要的数据呢？
Beautiful Soup 这个 Python 库，就是用来解决这一问题的最佳方法之一。


### 2. Beautiful Soup 是什么？
Beautiful Soup 是一个可以从 HTML 或 XML 文件中提取数据的 Python 库。它能够通过你喜欢的转换器，实现惯用的文档导航，查找，修改文档的方式。Beautiful Soup 会帮你节省数小时甚至数天的工作时间。


### 3. 安装
由于 Beautiful Soup 目前最新的版本是 Beautiful Soup 4，因此默认使用最新的版本。
###### 1) 安装 Beautiful Soup：
```
$ pip install beautifulsoup4
```

###### 2) 安装解析器：
Beautiful Soup 支持 Python 标准库中的 HTML 解析器，同时还支持一些第三方的解析器，不过为了快速了解 Beautiful Soup 的用法，因此使用的是默认 Python 标准库解析器，所以不需要另外安装，如果有需要使用到第三方的解析器，请到文章的末尾跳转去官方文档进行了解。

Python 标准库解析器用法： "html.parser"
```
BeautifulSoup(html_doc, "html.parser")      # Python 标准库， HTML 解析器
# 如果不指定解析器，则使用的是 Python 内部默认的解析器 "html.parser"
# soup = BeautifulSoup(html_doc)
```
### 4. 创建 Beautiful Soup 对象
```
html_doc = """
<!DOCTYPE html>
<html lang="en" dir="ltr">
<head><title>当幸福来敲门</title></head>
<body>
<div class="info">
<span class="title">当幸福来敲门</span>
<b><!-- 播放链接 --></b>
<a href="http://www.example.com">播放</a>
<p class="bd1">导演: 加布里尔·穆奇诺 Gabriele Muccino</p>
<p class="bd2">主演: 威尔·史密斯 Will Smith</p>
<span class="rating_num">9.1分</span>
<span>1228277人评价</span>
<p class="quote"><span class="inq">平民励志片。</span></p>
</div>
</body>
</html>
"""
```

使用 BeautifulSoup 解析这段代码，能够得到一个 BeautifulSoup 的对象
```
>>> from bs4 import BeautifulSoup
>>> soup = BeautifulSoup(html_doc, 'html.parser')
# >>> soup = BeautifulSoup(html_doc)
```


### 5. 四大对象种类
Beautiful Soup 将复杂 HTML 文档转换成一个复杂的树形结构，每个节点都是 Python 对象，所有对象可以归纳为 4 种:
###### 1) Tag（与原生文档中的 tag 相同）
Tag 是什么？通俗点说就是 HTML 中的一个个标签（默认提取匹配的第一个标签）
```
>>> soup.head
# <head><title>当幸福来敲门</title></head>
>>> soup.title
# <title>当幸福来敲门</title>
>>> soup.span
# <span class="title">当幸福来敲门</span>
>>> soup.span['class']
# ['title']
>>> soup.span.get('class')
# ['title']
```
######  2) NavigableString（可以遍历的字符串）
当我们获取了标签的内容后，想要获取标签内的文字，则使用 .string
```
>>> soup.title.string
# '当幸福来敲门'
```
###### 3) BeautifulSoup
BeautifulSoup 对象表示的是一个文档的全部内容。大部分时候，可以把它当作 Tag 对象，是一个特殊的 Tag 也就是上面的 soup 本身
```
>>> soup = BeautifulSoup(html_doc, 'html.parser')
# 该对象的属性
>>> soup.name
# '[document]'
```
###### 4) Comment（注释及特殊字符串）
Tag、NavigableString、BeautifulSoup 几乎覆盖了 html 和 xml 中的所有内容，但是还有一些特殊对象。也就是文档的注释部分:
Comment 对象是一个特殊类型的 NavigableString 对象，当它出现在 HTML 文档中时，Comment 对象会使用特殊的格式输出:
```
>>> comment = soup.b.string
>>> type(comment)
# <class 'bs4.element.Comment'>
>>> comment
# ' 播放链接 '
```

### 6. 搜索文档树
Beautiful Soup 定义了很多搜索方法，这里就着重介绍 2 个:
###### 1) find_all(name, attrs, recursive, text, **kwargs)
find_all () 方法搜索当前 tag 的所有 tag 子节点，返回结果是值包含一个元素的列表使用标签名来作为参数
```
>>> soup.find_all('a')
# [<a href="http://www.example.com">播放</a>]
>>> soup.find_all(['a', 'b'])
# [<b><!-- 播放链接 --></b>, <a href="http://www.example.com">播放</a>]
```
使用标签的属性来作为参数
```
class 因为是保留字，所以用 class 属性查找要改成 class_
>>> soup.find(class_='rating_num')
# <span class="rating_num">9.1分</span>
```
还可以使用正则表达式来作为参数
```
>>> import re
>>> soup.find_all(re.compile("^p"))
# [<p class="bd1">导演: 加布里尔·穆奇诺 Gabriele Muccino</p>, <p class="bd2">主演
# : 威尔·史密斯 Will Smith</p>, <p class="quote"><span class="inq">平民励志片。</
# span></p>]
```

###### 2) find(name, attrs, recursive, text, **kwargs)
它与 find_all() 方法唯一的区别是 find_all() 方法的返回结果是值包含一个元素的列表，而 find() 方法直接返回结果
```
>>> soup.find('b')
# <b><!-- 播放链接 --></b>
>>> soup.find_all('b')
# [<b><!-- 播放链接 --></b>]
```

### 7. CSS 选择器
在 BeautifulSoup 对象的 .select() 方法中传入字符串参数，即可使用 CSS 选择器的语法找到 tag:
Beautiful Soup 支持大部分的 CSS 选择器，.select() 方法返回的结果是匹配的数据的列表形式。
```
>>> soup.select("title")
# [<title>当幸福来敲门</title>]
>>> soup.select("div p")
# [<p class="bd1">导演: 加布里尔·穆奇诺 Gabriele Muccino</p>, <p class="bd2">主演
# : 威尔·史密斯 Will Smith</p>, <p class="quote"><span class="inq">平民励志片。</
# span></p>]
>>> soup.select("div > .quote")
# [<p class="quote"><span class="inq">平民励志片。</span></p>]
>>> soup.select("p:nth-of-type(2)")
# [<p class="bd2">主演: 威尔·史密斯 Will Smith</p>]
>>> soup.select("p:nth-of-type(2)")[0].string
# '主演: 威尔·史密斯 Will Smith'
>>> soup.select("p:nth-of-type(2)")[0].get_text()
# '主演: 威尔·史密斯 Will Smith'
```

  select 方法返回的结果都是列表形式，可以遍历形式输出，然后用 get_text() 方法来获取它的内容。
```
soup.a.get_text()
```

CSS 选择器参考手册:
        https://www.w3school.com.cn/cssref/css_selectors.asp

### 8. 总结
由于本篇文章是为了快速上手使用 Beautiful Soup 来提取数据，更多详细内容请查看官方文档。

官方文档：
        https://beautifulsoup.readthedocs.io/zh_CN/latest/
