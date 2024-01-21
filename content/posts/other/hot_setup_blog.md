+++
title = '建立自己的Blog'
date = 2024-01-22T00:45:56+08:00
draft = false
+++

> 拖延了2个月.... 终于开始建立自己blog了!

## Template Engine
Wordpress确实很美好, 但对于blog来说, 它太复杂了, 因为我并不想要数据库, 以及复杂的交互, 所以简单的静态网站就足够满足我的需求了. 因此, 这个blog选择了[Hugo](https://gohugo.io/)，它很简单, 写blog基本上就是 .md,  然后通过模板引擎把 .md 根据theme(hugo拥有非常丰富的[theme](https://themes.gohugo.io/)) render成html.


## Post Blog
写blog就和写普通的.md一样, 非常简单!

可以使用命令
```sh
hugo new content posts/[your blog].md
```

或者, 当了解到 [文件结构](https://gohugo.io/getting-started/directory-structure/)后, 自己手动创建也行.

当然, 有一个细节需要注意的是, 如果要public这篇blog, 需要把 draft设置为false(我就弄了半天, 没明白为什么本地调试可以看到新的blog, 结果发布就看不到, 结果发现是这个参数没设置正确)

```
draft = false
```

## Debug

```sh
hugo server -D
```
这个时候, 你本地修改完, Save后, 网站会实时刷新, 非常方便.


## Deploy

```sh
hugo
```
就是这么简单!


## Publish to the Internet
域名和vps选择了 [hostinger](https://www.hostinger.com/)

Http Server 选择了 Apache2(一开始选择了nginx, 但是没明白怎么设置root)
对于 Apache2 在conf 里 把 DocumentRoot 指向到 hugo 的 public
```sh
# /etc/apache2/sites-available/000-default.conf
<VirtualHost *:80>
	# The ServerName directive sets the request scheme, hostname and port that
	# the server uses to identify itself. This is used when creating
	# redirection URLs. In the context of virtual hosts, the ServerName
	# specifies what hostname must appear in the request's Host: header to
	# match this virtual host. For the default virtual host (this file) this
	# value is not decisive as it is used as a last resort host regardless.
	# However, you must set it for any further virtual host explicitly.
	#ServerName www.example.com

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/hugo/blog/public
    
	# Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
</VirtualHost>
```

修改完conf后, apply 下conf
```sh
# 检测下是否有语法错误
sudo apachectl configtest


# 重启Apache服务
sudo systemctl restart apache2
```


域名解析 可以看下[官网的教程](https://support.hostinger.com/en/articles/1583227-how-to-point-a-domain-to-your-vps), 设置完后, 会发现还是跳转到hostinger原来的地址, 说明dns缓存还在生效, 据说如果不手动flush, 要等24小时后, 才会刷新(ping 域名的时候, 返回值还是默认ip). 这个时候 可以换个网络(比如说手机流量)去访问, 就会发现会跳转到正确的ip了.


## Update Blog
因为直接在远端调试并不方便, 而且不好做备份, 所以我直接在本地创建了一个hugo rep, 这样比较方便调试和编写blog, 写完后push到github上, 最后远端只需要去pull之后, deploy一下就好.


## End
最后, 不得不说... 白白缴了两个月的网费, 才把自己blog搭起来 ORZ... 之后要开始更新下了!