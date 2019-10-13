1. 修改zabbix页面中文显示

	![](https://mmbiz.qpic.cn/mmbiz_png/4iaE7bB4HCjcfKbVLOZSsmtVk6icEhQkBFsuNuJYrFECvfZgTKJEty7KmWwb7WFOiaajj0DU04Rh4LdUiaHvHmIwPg/0?wx_fmt=png)

2. 压面乱码问题处理

    1. 下载字体 [simkai.ttf](http://www.font5.com.cn/tag.php?tag=simkai.ttf) 放到下面路径下 /usr/local/apache/htdocs/zabbix/assets/fonts
    
    2. 修改 vim /usr/local/apache/htdocs/zabbix/include/defines.inc.php 69行
    
        ```define('ZBX_GRAPH_FONT_NAME',           'simkai'); // font file name```