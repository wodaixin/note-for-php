### Web 资源防盗链



#### 什么是防盗链

盗链概念：

盗链是指在自己的页面上展示一些并不在自己服务器上的资源

获取他人服务器上的资源地址，绕过别人的资源展示页面，直接在自己的页面上向最终用户提供此内容

常见的是小站盗用大站的图片、音乐、视频、软件等资源

通过盗链的方法可以减轻自己服务器的负担，因为真实的空间和流量均是来自别人的服务器

防盗链概念：

防止别人通过一些技术手段绕过本站的资源展示页面，盗用本站的资源，让绕过本站资源展示页面的资源链接失效

可以大大减轻服务器及带宽的压力



#### 工作原理：

1. 通过 `Referer` 或者签名，网站可以检测目标页面访问的来源页面，如果哦是资源文件，则可以跟踪到显示它的网页地址。一旦检测到来源不是本站即进行阻止或者返回指定的页面 
2. 通过计算签名的方式，判断请求是否合法，如果合法则显示，否则返回错误信息。

 防盗链的实现方法：

`Referer` ：Nginx 模块 ngx_http_referer_module 用于阻挡来源非法的域名请求

Nginx 指令 valid_referers，全局变量 $invalid_referer

```config
valid_refers none | blocked | server_names | string ...;
```

none ： `Referer` 来源头部文件为空的情况下

blocked ：`Referer` 来源头部文件不为空，但是里面的值被代理或者防火墙删除了，这些值都不以 `http://` 或者 `https://` 开头

server_name ：`Referer` 来源头部文件包含当前的 `server_names`

```conf
location ~.*\.(gif|jpeg|png|flv|swf|rar|zip)$
{
	valid_referer none blocked imooc.com *.imooc.com;
	// $invalid_referer 合法为0 否则为1
	if($invalid_referer){
		$return 403
		rewrite ^/ http://www.imooc.com/403.jpg;
	}
}
```

可伪造Referer



可以使用加密签名解决

使用第三方模块 HttpAccessKeyModule 实现 Nginx 防盗链

accesskey on|off 模块开关

accesskey_hashmethod md5|sha-1 签名加密方式

accesskey_ary GET 参数名称

accesskey_signature 加密规则

```
location ~.*\.(gif|jpeg|png|flv|swf|rar|zip)$
{
	accesskey on;
	accesskey_hashmethod md5;
	accesskey_ary "key";
	accesskey_signature "mypass$remote_addr";
}
```

页面配置：

```php
<?php
    $sign = md5('mypass'.$_SERVER['REMOTE_ADDR']);
	echo '<img src="./logo_new.png?=' . $sign .'">';
```
