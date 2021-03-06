# python学习推荐

本人业余写python爬虫、python小工具已有两年有余了，在这里推荐一些学习时觉得比较好的视频教程、文章教程等，纯属个人建议，不喜勿喷

## 目录

-[资料篇](#资料篇)

-[爬虫篇](#爬虫篇)

-[数据分析篇](#数据分析篇)

## 资料篇

自学最好的还是动手操作，想用什么找什么，语法文章推荐[Python3菜鸟教程](https://www.runoob.com/python3/python3-tutorial.html)
视频教学[中国大学MOOC](https://www.icourse163.org/)，个人比较偏好嵩天老师的课程,有很多课程涉及到了Python语言程序设计、Python网络爬虫与信息提取、Python数据分析与展示、Python科学计算三维可视化等，可以说很全[嵩天老师所有课程](https://www.icourse163.org/u/songtian425?userId=4462001&_trace_c_p_k2_=62341c6d74af412580478b9722c76317)
，其次视频推荐[莫烦Python](https://morvanzhou.github.io/),里面有文章介绍和视频介绍，别人偏向与文章介绍，这样更快更节省时间

## 爬虫篇

介绍一下爬虫涉及到的第三方库及其相关文档
1.[urllib3](https://pypi.org/project/urllib3/)
2.[urllib3文档](https://urllib3.readthedocs.io/en/latest/)
3.[Requests文档](http://www.python-requests.org/en/master/)
4.[Beautiful Soup英文版](https://www.crummy.com/software/BeautifulSoup/bs3/documentation.html)
5.[Beautiful Soup中文版](https://www.crummy.com/software/BeautifulSoup/bs3/documentation.zh.html)
6.[正则表达式](http://www.runoob.com/regexp/regexp-tutorial.html)
7.[scrapy](https://scrapy.org/)
8.[scrapy文档](https://docs.scrapy.org/en/latest/)
9.[css选择器语法](#css选择器语法)
10.[re正则表达式语法](#re正则表达式语法)
11.[xpath语法](#xpath语法)
12.[PyMySQL](https://pypi.org/project/PyMySQL/)
13.[PyMySQL文档](https://pymysql.readthedocs.io/en/latest/index.html)

## 数据分析篇

数据分析的第三方库介绍
1.[Blaze](https://github.com/blaze/blaze)
2.[Open Mining](https://github.com/mining/mining)
3.[Orange](https://orange.biolab.si/)
4.[Pandas](http://pandas.pydata.org/)
5.[Optimus](https://github.com/ironmussa/Optimus)
6.[NumPy](http://www.numpy.org/)

附上github上python不同用途的第三方库的总价[awesome-python](https://github.com/vinta/awesome-python)

## css选择器语法

```
表达式                          说明
*                              选择所有节点
#container                     选择id为container的节点
.container                     选取所有class包含container的节点
li a                           选取所有li下的所有a节点
ul + p                         选择ul后面的第一个p元素
div#container > ul             选取id为container的div的第一个ul子元素

ul ~ p                         选取与ul相邻的所有p元素
a[title]                       选取所有有title属性的a元素
a[href="http://baidu.com"]     选取所有href属性为http://baidu.com值的a元素
a[href*="baidu"]               选取所有href属性包含baidu的a元素
a[href^="http"]                选取所有href属性值以http开头的a元素
a[href$=".jpg"]                选取所有href属性值以.jpg结尾的a元素
input[type=radio]:checked      选择选中的radio的元素

div:not(#container)            选取所有id非container的div属性
li:nth-child(3)                选取第三个li元素
tr:nth-child(2n)               第偶数个tr

[css视频介绍](http://www.w3school.com.cn/css/css_selector_type.asp)
```

## re正则表达式语法

```
字符                       匹配
.                       任意字符（除了\n）
[...]                   字符集
\d/\D                   数字/非数字
\s/\S                   空白/非空白
\w/\W                   单词字符[a-zA-Z0-9]/非单词字符
*                       前一个字符0次或者无限次
+                       前一个字符1次或者无限次
?                       前一个字符0次或者一次
{m}/{m,n}               前一个字符m次或者n次
*?/+?/??                非贪婪（尽可能少匹配字符）
^                       字符串开头
$                       字符串结尾
\A/\Z                   指定的字符串必须出现在开头/结尾
|                       匹配左右任意一个表达式
(ab)                    括号中表达式作为一个分组
\<number>               引用编号为num的分组匹配到的字符串
(?P<name>)              分组起一个别名
(?P=name)               引用别名为name的分组匹配字符串
[\u4E00-\u9FA5]         一个汉字
```

## xpath语法

```
表达式                     说明
article                   选取所有article元素的所有子节点
/article                  选取跟元素article
article/a                 选取所有属于article的子元素的a元素
//div                     选取所有div子元素（不论出现在文档任何地方）
article//div              选取所有输入article元素的后代的div元素，不管它出现在article之下的任何位置
//@class                  选取所有名为class的属性

/article/div[1]           选取属于article子元素的第一个div元素
/article/div[last()]      选取输入article子元素的最后一个div元素
/article/div[last()-1]    选取属于article子元素的倒数第二个div元素
//div[@lang]              选取所有拥有lang属性的div元素
//div[@lang='eng]         选取所有lang属性为eng的div元素

/div/*                    选取属于div元素的所有子节点
//*                       选取所有元素
//div[@*]                 选取所有带属性的title元素
//div/a|//div/p           选取所有div元素的a和p元素
//span|//ul               选取文档中的span和ul元素
article/div/p|//span      选取所有属于article元素的div元素的p元素以及文档中所有的span元素
```

