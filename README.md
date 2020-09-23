> 提醒： 滥用可能导致账户被BAN！！！ 

#### 部署Heroku空间

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/rickychaw/shadowsocks-libev) 

点击上面紫色`Deploy to Heroku`，会跳转到heroku app创建页面，填上app的名字、修改密码、加密方式、路径，当然用默认的也没问题。然后点击下面deploy创建APP，完成后会生成一个域名，到此服务端也就部署完成了。记下域名，后面客户端会用到。


### 客户端配置

首先下载 v2ray-plugin 插件到电脑，下载解压后移动到常用文件夹，记住插件所在文件夹路径，待会会用到。

v2ray-plugin 插件下载页：https://github.com/shadowsocks/v2ray-plugin/releases

然后，下载ss客户端，比如[Windows客户端](https://github.com/shadowsocks/shadowsocks-windows/releases/)，这个不多讲。配置如下：

* 服务器地址: shadowsocks-libev.herokuapp.com  //此处填写服务端生成的域名
* 端口: 443
* 密码：password
* 加密：chacha20-ietf-poly1305
* 插件程序：D:\APP\v2ray-plugin_windows_amd64.exe  //此处要填插件在电脑上的绝对路径
* 插件选项: path=/posts.html;host=shadowsocks-libev.herokuapp.com;tls //此处填上域名和路径

##### cloudflare workers反代

```
addEventListener(
    "fetch",event => {
        let url=new URL(event.request.url);
        url.hostname="appmane.herokuapp.com";         //将 appname 替换为你Heroku中App的名称
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
