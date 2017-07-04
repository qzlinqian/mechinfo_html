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

通过github备份所有网页文件和hexo文件。
在新电脑上，运行：
	
	git clone https://github.com/DME-info/mechinfoBlog.git
	cd mechinfoBlog
	npm install

运行以上命令，即可成功下载hexo整个文件夹，如果不运行`npm install`会导致hexo无法识别此文件夹，无法进行`hexo server`等操作。

特殊情况：有时，例如在李兆基的服务器上，我们利用dme登陆，但不在dme的用户文件夹创建文件，例如在/var/www目录下创建了文件。此时我们不具有超级用户权限，每条命令必须使用`sudo`前缀。这时需要修改文件权限才使得整个文件夹得以被操作。具体方法是：(一般在自己的电脑上不需要以下操作)
	
	sudo chown dme mechinfoBlog/ #将文件夹owner修改为dme
	sudo chown dme -R mechinfoBlog/ #将文件夹内所有文件的owner修改为dme
	chmod ug=rwx,o=r mechinfoBlog/ #给dme和同组的人都赋予读写和操作文件夹的权限
	chmod ug=rwx,o=r -R mechinfoBlog/ #文件夹内的所有文件执行上一行操作
天坑：如果你执行了以上操作，会造成git认为所有文件被改动过（锅主要是第四句话）。git会默认读取文件的模式，这样做的后果是会和远程版本冲突。如果你同时在其他地方不小心改了本地文件，修改过db.json并push到github上了，此时再想将这里的db.json push上去就不能成功。因此，我们要在文件夹内修改git的配置，让git忽略文件的mode属性。为了达到这个目的你必须从头来过，git clone之后立刻在文件夹内执行
	
	git config core.filemode false
此后，你可以任意修改文件权限而不引起git的不适。

另：具体修改文件权限的语句含义，可以自己查书或网站。使用`ls -l`命令查看你想要看的文件权限，确保你自己的账户可以对这些文件进行读、写、操作。
	

## 部署方法
---

注意事项！

* 每次在自己的电脑上操作前，使用`git pull`确保本地版本和github上的版本完全一致！
* 每次在自己的电脑上修改完成后，最后一步一定要用`git status`查看确认本地和远程同步了。在李兆基的server上也要进行一样的操作。如果`git status`出现大量修改过的文件，则意味着某一步出错，需要进行手动的筛查。记得做备份。
* 文件组织发生重大改动。以后服务器都是由apche显示静态网页，而不是显示hexo server在4000端口显示的内容。github上同时维护了两个仓库，具体见下。
* 文件路径已经发生变动，现在服务器上显示的网页对应的地址是/var/www/mecinfo_html，hexo本身的文件夹是/var/www/mechinfoBlog ,hexo.old文件夹基本没用了，纯粹不忍心删掉。
* 可以参见https://www.digitalocean.com/community/tutorials/how-to-create-a-blog-with-hexo-on-ubuntu-14-04 ，这个教程内，脚本自动git pull的步骤我没做（因为懒。。）所以现在每次还是要手动git pull。另外，这个教程用的nginx而我们用的是apache。强烈建议先看一下这个链接，了解一下我们现在的部署方案。


在自己电脑的linux上命令行（假设你已经有这样一个和远端同步的仓库了）

	cd mechinfoBlog
	git pull
	hexo server

用浏览器查看本机上4000端口

	http://localhost:4000

接下来，可以进行一堆修改操作，commit了之后，在自己电脑上：

	hexo generate && hexo deploy #生成静态网页，在/public目录下
	
由于hexo已经被设置好了git备份模式，此时，你会被询问github账号和密码，照做之后public文件夹下的静态界面就会被自动上传到github上mechinfo_html这个仓库内（注意不是mechinfoBlog!）。回到服务器，在服务器上找到mechinfo_html这个文件夹，运行`git pull`，从而可以直接改变服务器显示的页面。

理论上，服务器没必要在弄一个mechinfoBlog的文件夹了，因为服务器只需要静态页面，但是作为备份仍然用着它吧。

如果你是在服务器上对mechinfoBlog进行修改操作，在另外一台电脑的浏览器输入old.mechinfo.me可以查看服务器的4000端口。也就是hexo server默认的输出端口。以后old.mechinfo.me就作为临时在线debug的端口，而不会影响mechinfo本身的运行。但我们仍建议在自己的电脑上debug好了再上传。

总结：`hexo server`仅仅是临时用来debug的，想要给用户显示，需要另外部署静态网页！在服务器上，apache已经被设置为链接到 /var/www/mechinfo_html 这个文件夹。如果需要修改apache服务器的配置，请前往 etc/apache2/sites-available/mechinfo.me.conf 修改这个文件，具体方式参考apache自己的相关文档。

## 发表文章方法
---
需要发表文章时，请确认**tag, categories**以及是否置顶

置顶文章请修改top优先级（默认值为0）

	lowest 0->1->2 highest
	
在front matter里加入 `<!-- more -->` 可以实现“阅读更多”功能。

## 修改网页配置方法
---
目前已经可以使用相对链接引用本地图片。使用

	![name](link)

来插入。

## ERROR LOG
---
* categories栏无法正常显示的问题：相关文件位置为./themes/landscape/layout/_partial/post,似乎重新hexo generate可使其恢复正常，具体原因待探究233

	2017.7.4 update：上述问题已解决。hexo server只是临时用来debug的方法，不应作为服务器运行的方案！hexo server不稳定，不能够正常显示categories栏。但生成静态html文件，利用apache2显示即可，不会再出现问题。

* ubuntu不知为何更新了或者怎么样了，会报错system problem detected，然后断网。需要物理上前往李兆基，手动执行以下操作：
	
		sudo rm /var/crash/*
		sudo reboot
		
	此错误原因有待探究！

