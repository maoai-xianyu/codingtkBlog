---
title: Hexo Mac和Git搭建个人博客以及博客如何绑定自己的域名
date: 2018-11-30 17:58:10
tags: 
	- Hexo
	- Git
	- Blog
categories: "Hexo"
---

## Hexo Mac和Git搭建个人Blog以及绑定自己的域名流程 

在Mac上使用Hexo搭建自己的Blog以及配置自己的域名，开始学习写Blog，记录自己的学习。

<!--more-->

### 一、准备工作
1. Hexo  https://hexo.io/zh-cn/docs/

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

2. 安装前提

   1. Mac 安装 brew 
   
   ```
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```
   
   2. 搭建Node.js环境
   
   ```
   brew install node 安装
   npm -v 查看版本
   ```
   
   3. 安装Hexo博客框架工具
   
   ```
   npm install hexo-cli -g  安装
   
   hexo 检查命令是否可用
   ```
   
   4. Mac 安装Wget 用于下官网 hexo 的稳定版主题
   
   ```
   brew install wget --with-libressl 安装
   brew link wget 查看是否可用
   ```
   
   5. Git 和 GitHub 账号

### 二、搭建博客 切换到 https://github.com/ 
如下举例说明：maoai-xianyu 为例，你需要设置自己的blog

1. 创建一个github仓库

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/FD4C032F0E7C45FD96A78FEC5F53E2E6?ynotemdtimestamp=1545734633728)

2. 点击项目的settings去配置Github Pages

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/995BBB8F4144484690CD2E8E715BC004?ynotemdtimestamp=1545734633728)

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/A4BEA65DD99148CD9A18A176A14C03B8?ynotemdtimestamp=1545734633728)

3. 选择一个theme,之后变成

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/D405D2C7C001444A9D8DB5EEE98F85A4?ynotemdtimestamp=1545734633728)

4. 如是我们可以 https://maoai-xianyu.github.io/ 访问自己的博客，当然目前只是默认网站，也是以后我们要把自己的写的blog发布到的地方。

### 三、配置本地博客站点

1. 进入自己的mac目录

```
hexo init codingtkBlog
```

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/3002AC3044544197B6D43E335C60B947?ynotemdtimestamp=1545734633728)

```
hexo g  //g是generetor的缩写，生成博客

hexo s  //s是server的缩写，启动服务

浏览器输入 http://localhost:4000/ 可以看到hexo默认主题显示的博客样式
```

2. 同步Github,允许公共访问

在github上，获取博客的地址 https

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/CDA7A7F701944B05B879AD371F432626?ynotemdtimestamp=1545734633728)

然后在codingtkBlog根目录下的 _config.yml 文件，修改 deploy 下的配置

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/A3757381A6094C10899CB0AAF3602F24?ynotemdtimestamp=1545734633728)

在github上，获取博客的地址 SSH

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/A3757381A6094C10899CB0AAF3602F24?ynotemdtimestamp=1545734633728)

然后在codingtkBlog根目录下的 _config.yml 文件，修改 deploy 下的配置

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/6FA4F7C15594453D9475C07AEF67FB5D?ynotemdtimestamp=1545734633728)


```
题外话:
1. 不喜欢mac自己的终端，可以下载一个 item2 可以有更好的体验
关于生成 SSH KEY
2. 输入 .ssh 
   2.1 如果有，就执行  vim id_rsa.pub 可以查看对应的公钥，然后将其放入 github
   2.2 如果没有，就 mkdir ~/.ssh  然后 ssh-keygen -t rsa -C "your_email@example.com"  cat id_rsa.pub 查看公钥  或者用  vim id_rsa.pub 显示公钥，然后将其放入 github
```

最后执行命令

```
npm install hexo-deployer-git —save //安装部署插件
hexo d //部署到github

访问 https://maoai-xianyu.github.io/ 就和本地 http://localhost:4000/ 显示的一样了

```

### 四、修改博客主题
1. 创建next文件夹

```
mkdir themes/next  
```

2. 下载主题Next目前比较流行，也可以自己选择 https://github.com/hexojs/hexo/wiki/Themes

```
$ curl -s https://api.github.com/repos/iissnan/hexo-theme-next/releases/latest | grep tarball_url | cut -d '"' -f 4 | wget -i - -O- | tar -zx -C themes/next --strip-components=1

这里的 wget 如果提示没有，就需下载 看准备工作的2
```

3. 修改博客配置文件，更换主题配置

修改博客根目录(不是next主题)下的_config.yml文件，搜索theme字段，并将其值修改为next。

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/2DF3DB654EE74FD4B080688250BF7643?ynotemdtimestamp=1545734633728)

```
hexo clean  //清理缓存

hexo g    //重新生成博客代码

hexo d   //部署到本地

https://maoai-xianyu.github.io/  切换主题
```

### 五、网站美化

对应的显示没有刻意的研究，目前可以参考一些大神

1. [hexo的next主题个性化教程](https://www.jianshu.com/p/f054333ac9e6)
2. [Hexo设置主题以及Next主题个性设置](https://www.jianshu.com/p/b20fc983005f)

### 六、管理博客在不同的电脑

我们将我们本地生成的blog，提交的gihub，那么就可以在不同的电脑上去整理了。

### 七、配置自己的域名
我的是腾讯的域名  www.codingtk.com

1. 打开腾讯云控制后台，点击域名查看对应的信息

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/2D8C4DF6D6FF46558054571706EDA8A9?ynotemdtimestamp=1545734633728)

2. 配置域名解析

![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/2228B982C03F40E3B8EE8DF1EF42B3DC?ynotemdtimestamp=1545734633728)

3. 在本地博客目录下创建 CNAME 文件
    1. 在 odingtkBlog/public 创建 CNAME 文件
    ![image](http://note.youdao.com/yws/public/resource/5bcd7e6742690f507beef37b094e68c7/F5C1D8938A2C4490B94E8865FB67F412?ynotemdtimestamp=1545734633728)
    2. 打开 CNAME 文件 添加域名 www.codingtk.com
    3. 发布CNAME
    ```
    hexo g  //重新生成博客代码
    hexo d // 发布到github
    ```
    4. 访问 [我的域名](https://www.codingtk.com/) 和 访问 [我的Blog](https://maoai-xianyu.github.io) 是同一个地方了

### 总结

学习，需要很努力的坚持。加油