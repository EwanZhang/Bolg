---
layout: post
title: 让GitHub Pages博客支持百度搜索引擎收录
categories:
- bolg
tags:
- centos
- GitHub
---


GitHub Pages搭建的网站是被系统禁止百度抓取的，要想解决这个问题，只有买个VPS自己撘一个jekyll解析博客了。但是域名没有备案，单纯用国外的VPS，用户访问起来也会有慢的问题，毕竟Github Pages的CDN还是很牛逼的。那有没有什么办法，让百度爬虫去抓国外VPS上的内容，普通用户直接访问Github Pages呢？

DNSPOD支持这个功能，免费用户可以 按电信、联通、教育网、百度、搜索引擎等分别设置解析，付费用户可以分省、分国家、分大洲等等。对于小博客来说，免费的就非常够用了。

环境：centos6

首先安装[Jekyll](http://www.ewanzhang.com/2015/07/centos6%E5%AE%89%E8%A3%85Jekyll/)  

**克隆博客代码**  
`sudo yum install git`

`git clone https://github.com/EwanZhang/EwanZhang.github.io.git ./mydomain.com`

**定时更新博客代码**  
配置crontab 2分钟pull一下代码：  
`crontab -e `

`# 添加下面的代码：`  
`*/2 * * * * cd ~/mydomain.com/; git pull >> /tmp/github_blog_pull_record.log 2>&1;`

Jekyll默认的markdown解析器maruku对中文支持不够完善，所以换成RDiscount解析器
`gem install rdiscount`

**启动博客站点**
Jekyll 2.4 版本之后，会关注文件的变动，自动重新生成静态文件。使用--detach参数指定在后台执行，--port指定端口，--host指定当前机器的外网ip。  
cd ~/mydomain.com/  
jekyll serve --port 80 --host 192.168.1.101 --detach

绑host 测试，能正常访问则搭建完成。

**配置智能dns**

登录DNSPOD，进入域名解析的设置页面，添加一条针对百度的解析：

![](/media/pic2015/2015-07-01-1.jpg)


参考原文链接：http://tabalt.net/blog/make-blog-support-baidu-search-engine/
