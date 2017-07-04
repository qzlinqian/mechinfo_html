# mechinfoBlog
---
Blog for mechinfo
- [x] 可以通过github同步
- [x] 不需操作数据库
- [x] 代码以及数据可以打包，方便迁移
- [x] 轻量，方便安装

## 解决方案
---
以上要求可以通过静态网页生成器达成
现行的两种方案 jekyll和hexo
http://blog.giacomocerquone.com/jekyll-vs-hexo/
jekyll我用过，需要使用ruby，代码确实管理混乱，非常难以理解。
推荐使用Hexo。

## 部署方法
---
在linux上命令行

	git clone https://github.com/DME-info/mechinfoBlog.git
	cd mechinfoBlog
	hexo server

用浏览器查看本机上4000端口

	http://localhost:4000

## 发表文章方法
---
需要发表文章时，请确认**tag, categories**以及是否置顶

置顶文章请修改top优先级（默认值为0）

	lowest 0->1->2 highest

## 修改网页配置方法
---
目前已经可以使用相对链接引用本地图片。使用

	![name](link)

来插入。

## FAQ
---
categories栏无法正常显示的问题：相关文件位置为./themes/landscape/layout/_partial/post,似乎重新hexo generate可使其恢复正常，具体原因待探究233

