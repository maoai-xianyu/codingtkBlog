---
title: Charles安装与破解
date: 2018-12-25 18:19:26
tags:
	- Charles
	- 抓包
categories: "Charles"
---

## Charles Proxy License

#### 参考 破解  [原文](https://blog.csdn.net/qq_25821067/article/details/79848589)

1. 适用于Charles任意版本的注册码，谁还会想要使用破解版呢。
2. Charles 4.2目前是最新版，可用。

<!--more-->

### 内容

1. Registered Name: https://zhile.io
2. License Key: 48891cf209c6d32bf4

## 使用 Charles 获取 https 的数据

#### 参考： [使用 Charles 获取 https 的数据](https://blog.csdn.net/yangmeng13930719363/article/details/51645435)

以上面为依据，设置自己的属性

1. ![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/F6FC69964E4A4751BFFA9FDEB7250020?ynotemdtimestamp=1545733796855)

2. 勾选Enable SSL Proxying,点击添加，弹出下面的对话框，Host 表示你要抓取的 ip 地址或是链接
    1. host 输入 \*.\*
    2. port 输入 *
    3. 如图
![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/97B13B97080F43B0BC027A3BED9D51C1?ynotemdtimestamp=1545733796855)

### Android 手机 小米为例子


修改  

1. 先设置局域网，保证手机和电脑在同一个网络下
![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/28683995C07B4954A34AF653A70EC240?ynotemdtimestamp=1545733796855)
2. 在QQ浏览器下输入  chls.pro/ssl 直接访问地址注意不是搜索
![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/7A09FB78CAF6419FB33408358CFABE66?ynotemdtimestamp=1545733796855)
3. 然后下载证书
![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/2E17A3F511B349DD9DC1BF56210ACB3E?ynotemdtimestamp=1545733796855)
![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/97DC5526B3384C878A4D0507C9DA5C79?ynotemdtimestamp=1545733796855)
4. 到小米手机  设置 -> 更多设置 -> 系统安全 -> 从存储设备安装 -> QQBrowser -> 其他 -> 点击下载证书安装

![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/7BBF7866EA924588B3730C48C72B0C00?ynotemdtimestamp=1545733796855)

5. 证书安装，需要设置手机锁屏密码，一定要设置哦。然后在charles进行测试
![image](http://note.youdao.com/yws/public/resource/d3a9407fc127670be709f5daa4e860a2/608D79A48ECA43379C0733A2DE87CA8E?ynotemdtimestamp=1545733796855)