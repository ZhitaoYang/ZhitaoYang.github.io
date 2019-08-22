---
title: Markdown 语法的简要规则
date: 2019-07-20 10:43:25
copyright: true
permalink: 1
tags: Markdown
categories:
password:
---
# Markdown 语法的简要规则
<!-- more -->

## 标题
在 Markdown 中，如果一段文字被定义为标题，只需要在这段文字前加#号即可，总共可有六级标题，即######
## 列表
无序列表只需在文字前加上-或*，有序列表则加上1. 2. 3.符号和文字间加空格
* 1 无序 1
- 2 无序 2
1. 有序 1
2. 有序 2
## 引用
只需要在文本前加入 > 这种大于号即可
> 引用例子
## 图片与链接
### 插入图片：
![Baidu](http://baidu.com)
插入图片需要插入图片的URL地址! [ ] ( )
### 插入链接：
[Baidu](https://www.baidu.com/)
比图片少一个!号 [ ] ( )
## 粗体和斜体
两个 * 包含文本是粗体，一个 * 包含一段文本是斜体

**粗体** *斜体*
## 表格
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

## 代码框
只需要三个`在首尾行把中间的代码包裹起来

```
# 1 获取索引界面网页内容
 def get_page_index(i):
    # 下载1页
    # url = 'https://www.thepaper.cn/newsDetail_forward_2370041'
    # 2下载多页，构造url
    paras = {
        'nodeids': 25635,
        'pageidx': i
    }
    url = 'https://www.thepaper.cn/load_index.jsp?' + urlencode(paras)
    response = requests.get(url,headers = headers)
    if response.status_code == 200:
        return response.text
        # print(response.text)  # 测试网页内容是否提取成功ok 
```

## 下划线
三个 * 号
***
