---
order: 4
---

# FAQ

## M1 芯片 Mac 如何安装

`Homebrew`已支持在`arm`上安装，但是一些软件兼容性还有待更新，官方查阅地址可看 [说明](https://github.com/Homebrew/brew/issues/7857) 。

你可以考虑安装`x86`版本保持兼容性。

无论安装哪个版本，强烈建议先参考文档：[M1 芯片 Mac 上 Homebrew 安装](/guide/m1/)。

大部分问题都是资源不稳定导致的，如果可以的话尽量通过代理加速。

代理使用可以参考以下文章：

- [Mac 终端走 ssr 代理](http://onwaier.com/?p=229)
- [如何让 macOS 的「终端」走代理？](https://www.xiaodefa.com/digital/121.html)

## fatal: unable to access xxx

```
HEAD is now at 028e733e7 Merge pull request #10260 from reitermarkus/audit-livecheck

fatal: unable to access 'https://github.com/Homebrew/homebrew-core/': transfer closed with outstanding read data remaining
fatal: ambiguous argument 'refs/remotes/origin/master': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
```

解决方案：

- 根据文章 [GitHub 访问加速指南][hosts] ，给`github.com`域名加速
- 通过代理访问

## 设置镜像无效，安装还是很慢

这里说的安装很慢主要是指`brew`本身安装速度慢（镜像地址没有设置成功）。

可以执行命令`echo $SHELL`，先查看终端类型。

- bash: `/bin/bash`
- zsh: `/bin/zsh`

然后根据终端类型设置镜像，参考"设置镜像"。

## command not found: brew

一般是环境变量无效，请使用`echo $SHELL`确认终端类型，参考上节。

如果是`m1`芯片`Mac`则可能是没有把`/opt/homebrew/bin`加入环境变量：

zsh

```shell
echo export PATH=/opt/homebrew/bin:$PATH >> ~/.zprofile
source ~/.zprofile
```

bash

```shell
echo export PATH=/opt/homebrew/bin:$PATH >> ~/.bash_profile
source ~/.bash_profile
```

否则可能因为特殊情况需要把`/usr/local/bin`手动加入环境变量：

zsh

```shell
echo export PATH=/usr/local/bin:$PATH >> ~/.zprofile
source ~/.zprofile
```

bash

```shell
echo export PATH=/usr/local/bin:$PATH >> ~/.bash_profile
source ~/.bash_profile
```

## SHA256 mismatch

```
Error: An exception occurred within a child process:
  ChecksumMismatchError: SHA256 mismatch
Expected: 01059532335fefc5e0e7a23cc79eeb1dc6fea477606981b89f259aa0e0f9abc1
  Actual: 641ba394c891cee16dfa45873906edab12edfc9befaa121649bb18e7b7d574bf
 Archive: /Users/XXX/Library/Caches/Homebrew/downloads/1124b500fc261f6d9be6193eca8c120f4dfb7ecd3470ac7d8edc1d04ecc5b345--jdk13u-jdk-13.0.2+8.tar.bz2
To retry an incomplete download, remove the file above.
```

遇到该问题时请尝试该方案：https://blog.csdn.net/lemostic/article/details/107101219

## 如何判断终端类型

在终端执行命令`echo $SHELL`，根据显示的结果判断：

- `/bin/bash` => `bash` => `.bash_profile`
- `/bin/zsh` => `zsh` => `.zprofile`

## Xcode Command Line 安装问题

错误如下：

```
xcode-select: error: invalid developer directory '/Library/Developer/CommandLineTools'
Failed during: /usr/bin/sudo /usr/bin/xcode-select --switch /Library/Developer/CommandLineTools
```

这是因为`CommandLineTools`未安装的原因，可以通过命令安装：

```shell
xcode-select --install
```

如果提示"不能下载该软件，因为网络出现问题"，可以在官网下载安装。

1. 打开 https://developer.apple.com/download/more/
2. 搜索 Command Line

因为这里罗列的所有的版本，所以我们还需要确定支持的版本，我们这样操作：

1. 确定当前系统版本
2. 打开`App Store`搜索`Xcode`
   ![WX20210218-184737@2x.png](https://i.loli.net/2021/02/18/9hWIP8sQ7tYFOvS.png)

3. 点击"版本历史记录"找到支持你的系统版本的`Xcode`版本号
   ![WX20210218-184755@2x.png](https://i.loli.net/2021/02/18/4d1wtirp8Wxky5e.png)

举例子我的电脑是`Big Sur 11.2`，在版本记录里看到`12.4`就是`Xcode`对应的版本号，然后我们找到"Command Line Tools for Xcode 12.4"下载。

[hosts]: https://mp.weixin.qq.com/s/gFNP2Pk81vg7nE1XsDingg