# django模板层

### 模板语法
{{  }}	-	变量相关<br>
{%   %}	-	逻辑相关

传递类名时，模板语法也会帮你加括号调用，实例化得到一个对象。
 数据取值可以通过**句点符**获取，不管是索引还是键都可以。<br>
![models](img\models.png)
![models-view](img\models-view.png)
*模板语法不能给函数传参，模板语法不支持给函数传参*



#### 过滤器
{{ 数据|方法:参数 }}
**length：** 统计字符串长度<br>
**slice：'参数'** - 切片操作	参数(开始下标:结束下标:步长）<br>
**default：'参数'** - 默认值，传递一个参数(如果前面的数据有值-为True，那就输出前面的值，如果没有-False，则输出参数的值)<br>
**filesizeformat：'参数'** - 将数字转换成文件大小的格式<br>
**data：**日期格式化	Y-年 m-月 d-日 h-时 i-分 s-秒<br>
**truncatechars：'参数'** - 截取字符串，对文字进行截取，其他字符用省略号代替，参数长度包含省略号三个点。<br>
**truncatewords：'参数'** - 截取单词,对英语单词进行截取(实际是按照按照空格进行截取，不包含省略号)<br>
**join：'参数'** - 拼接<br>
**safe：**转义<br>
``` python
# 后端直接转义
from django.utils.sagestring import mark_safe
	html = "<h1>hello</h1>"
	res = mark_safe(html)
```
![filter](img\filter.png)
![filter-view](img\filter-view.png)

#### 标签
**for循环：**专属变量名*forloop* （endfor结束）
**if：**条件语句				（endif结束）
![tag](img\tag.png)
![tag-view](img\tag-view.png)

### 自定义
``` python
# 1.现在应用下创建一个名字叫templatetags文件夹
# 2.再在该文件夹中创建任意名字的py文件
# 3.再在该py文件中必须书写一下两句话
	from django import template
	register = template.Library()
```
####  自定义过滤器
``` python
    @register.filter(name='hello')
    def mysum(v1, v2):
        return v1 + v2
```

####  自定义标签
``` python
    @register.simple_tag(name='plus')
	def test(a, b, c, d):
    	return '%s-%s-%s-%s' %(a, b, c, d)
```
![custom-modes](img\custom-modes.png)
![custom-modes-view](img\custom-modes-view.png)

####  自定义inclusion_tag
``` python
# 先定义一个方法，在页面上调用该方法，并且可以传值
# 该方法可以生成一些数据然后传递给自定义html页面
# 之后调用的就是渲染好的页面
    @register.inclusion_tag('left_menu.html')
    def left(n):
        data = ['第{}项'.format(i+1) for i in range(n)]
        return locals()
```
``` html
<ul>
    {% for foo in data %}
        <li>{{ foo }}</li>
    {% endfor %}
</ul>
```
![inclusion_tag](img\inclusion_tag.png)
![inclusion_tag-view](img\inclusion_tag-view.png)
*当html页面某一个地方需要传递参数才能动态渲染出来，并且很多页面都需要用到这些数据，就可以搓成inclusion_tag的形式*

### 模板继承
{% extends 'html文件' %}:继承文件所有内容<br>
如果需要需要修改某块内容，可以提前划分好区域：<br>
{% block 区域名 %}<br>
{% endblock %}<br>
![extends](img\extends.png)
![extends-view](img\extends-view.png)

``` html
<head>
    {% block css %}
	{% endblock %}
</head>
<body>
    {% block 区域名%}
	{% endblock %}
</body>
    {% block js %}
    {% endblock %}
```
1、先选择好一个想要继承的模板页面<br>
{% extends 'html文件'  %}<br>
2、继承之后子页面和模板长得一样<br>
如果需要需要修改某块内容，可以提前划分好区域：<br>
{% block 区域名 %}<br>
{% endblock %}‘<br>
3、页面使用的时候直接通过block划分来使用<br>
{% block 区域名 %}<br>
{% endblock %}‘<br>