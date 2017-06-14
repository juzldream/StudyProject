<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
		<link rel="stylesheet" href="./showdown.css"/>
		<link rel="stylesheet" href="./styles/github.css">
		<script type="text/javascript" src="./highlight.js"></script>
		<script type="text/javascript" src="./showdown.js"></script>
		<script type="text/javascript" src="./showdown-toc.js"></script>
	</head>

	<body>
		<textarea id="text-input" style="display:none"># demo


# 标题1

标题1: with =
===

## 标题2

标题2: with = 
---

### 标题3
#### 标题4
##### 标题5
###### 标题6



# 段落
我现在开始学习GitHub 和 Makedown，     我的微信订阅号是：juzldream  

# 强调
我是 **粗体** ，我是 *斜体* ，我是 ***加粗倾斜*** ，我是~~删除线~~


### 安装 GFM viewer 插件

# 无序列表

- 任务清单
	- Linux 学习
		* Samba
		* OpenLDAP
	- Windos 学习

# 有序列表

1. 文化课
	1. 英语
	2. 数学
	3. 语文
2. 其它
	1. 体验</textarea>
		<div id="preview" class="markdown-body"> </div>
		<script type="text/javascript">
			var $ = function (id) { return document.getElementById(id); };
			var converter = new showdown.Converter({ extensions: ['toc'], 'tasklists':true, 'tables':true});
			$("preview").innerHTML = converter.makeHtml($("text-input").value);
			hljs.initHighlighting();
		</script>
	</body>
</html>
