---
title: Mac上Gradle安装与安装其他版本
date: 2018-12-26 15:13:14
tags:
	- gradle
	- gradle version
categories: "Gradle"
---

## Mac上Gradle安装与安装其他版本

报错：==org.gradle.api.tasks.compile.CompileOptions.setBootClasspath(Ljava/lang/String;)V==

报错：==Android Studio Debug模式调试失败==

<!--more-->

文章原因：

1. 原因一：gradle的版本过高导致Android项目上的gradle打包命令失败，

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/7387B8D541C240F89F46558FCD9FC39F?ynotemdtimestamp=1545808939354)

2. 原因二：在使用本地的gradle，导致debug调试失败

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/ADFC625150C94F64954A50767F1783C2?ynotemdtimestamp=1545808939354)


### 准备工具

1. 在mac上下载[iTerm2](https://www.iterm2.com/)，用于命令终端
2. 在Mac上下载[brew package manager](https://brew.sh/)

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 下载Gradle

1. brew install gradle 下载gradle
2. 下载成功后执行
    1. brew info gradle 查看gradle的信息
    
    ![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/F13D70CA079C46D8BFE87E7206532830?ynotemdtimestamp=1545808939354)
    
    2. gradle -v 查看gradle的版本信息
    
    ![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/2218B44C153A4AAD92EB69040E25A455?ynotemdtimestamp=1545808939354)
    
3. 执行上面的命令，下载的是目前**brew**中最新的gradle版本，我的目前是 [gradle 5.0](https://github.com/Homebrew/homebrew-core/blob/master/Formula/gradle.rb)

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/597CB372686F4071BFBE11B4AFBFD8C9?ynotemdtimestamp=1545808939354)

### 下载其他版本的gradle

下载其他版本的 **gradle** 是为了可以解决上面的报错

1. 下载需要安装的[gradle版本](https://gradle.org/releases/)，我这里是 [gradle 4.6](https://gradle.org/next-steps/?version=4.6&format=bin)

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/1DEC0724364B455F8C469CCC06FA3B5E?ynotemdtimestamp=1545808939354)

2. 查看文件sha256

```
openssl dgst -sha256 [gradle-4.6-all.zip的路径，可以直接拖进来]

gradle 4.6对应的 sha256 是 9af7345c199f1731c187c96d3fe3d31f5405192a42046bafa71d846c3d9adacb
```

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/11A6E3CAF1064470B1ACBA10C981B1C8?ynotemdtimestamp=1545808939354)

3. 下载brew的源码 [homebrew-core](https://github.com/Homebrew/homebrew-core)
4. 在**homebrew-core** -> Formula -> 找到**gradle.rb**文件，copy一份出来，进行修改

```
class Gradle < Formula
  desc "Open-source build automation tool based on the Groovy and Kotlin DSL"
  homepage "https://www.gradle.org/"
  url "https://services.gradle.org/distributions/gradle-4.6-all.zip"
  sha256 "9af7345c199f1731c187c96d3fe3d31f5405192a42046bafa71d846c3d9adacb"

  bottle :unneeded

  option "with-all", "Installs Javadoc, examples, and source in addition to the binaries"

  depends_on :java => "1.7+"

  def install
    rm_f Dir["bin/*.bat"]
    libexec.install %w[bin lib]
    libexec.install %w[docs media samples src] if build.with? "all"
    (bin/"gradle").write_env_script libexec/"bin/gradle", Language::Java.overridable_java_home_env
  end

  test do
    assert_match version.to_s, shell_output("#{bin}/gradle --version")
  end
end

```

5. 在iterm2中执行文件 gradle.rb

```
brew install /Users/zhangkun/zk_develop/gradle/gradle.rb
```

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/E6B9E5834AA1482F9B0682F874976FF8?ynotemdtimestamp=1545808939354)

6. 提示error后执行建议的命令 brew unlink gradle 减除关联

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/1785FE1D055E45DD87786DD99B0F4C28?ynotemdtimestamp=1545808939354)

```
brew unlink gradle
```

7. 然后继续第5步骤

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/A692B4615FF54C7695B5183B7EAE056C?ynotemdtimestamp=1545808939354)

8. 执行 brew info gradle 查看安装的gradle的版本

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/95653F86369845CC9201B9BA5D43CBBA?ynotemdtimestamp=1545808939354)

9. 切换版本  brew switch gradle 4.6

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/9DEA551D338342C7A13C0EB554DDA339?ynotemdtimestamp=1545808939354)

10. 查看当前使用的版本 gradle -v

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/6B6EBE5033104935909EC99CCD078289?ynotemdtimestamp=1545808939354)


### 结束

1. 之后用gradle打包ok
2. 在Android Studio 中修改本地环境

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/FD7A82A5E5AB448BBD3D2E9A1323536E?ynotemdtimestamp=1545808939354)

![image](http://note.youdao.com/yws/public/resource/a9600243aef545516065218cc469e3da/4E4DE6A1AB4741BBB6AACDD9D3F5010B?ynotemdtimestamp=1545808939354)

3. 本文参考 [homebrew 安装指定版本gradle（软件）](https://www.jianshu.com/p/a537d9a4034f)

