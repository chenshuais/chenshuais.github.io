在你的安卓设备上通过 Termux 和 Ollama 释放大型语言模型的力量在这篇博客文章中，我们将探讨如何使用强大的终端模拟器 Termux 在安卓设备上安装和运行 Ollama 语言模型。本教程是为那些希望直接在他们的移动设备上利用大型语言模型的能力而无需桌面环境的用户设计的。

## 1. 安装 Termux 

下载并安装：https://f-droid.org/repo/com.termux_117.apk

## 2. 更新软件包

打开 Termux ，输入以下命令：
```bash
pkg update
pkg upgrade
```

## 3. 安装 Proot-Distro 

[Proot-Distro](https://github.com/termux/proot-distro) 可以在 Termux 内运行不同的 Linux 版本，比如 Ubuntu、Debian 等

```bash
pkg install proot-distro
```

## 4. 下载 Debian 系统
```bash
pd install debian
```

## 5. 登录到 Debian 系统上
```bash
pd login debian
```

## 6. 更新 Debian 系统内的软件包
```bash
apt update
apt upgrade
```

## 7. 安装 Ollama
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

## 8. 运行 Ollama 服务
```bash
ollama serve
```

## 9. 开启一个新的 Termux 会话

回到手机桌面，长按 Termux 图标，选择 `New session`

## 10. 在新的会话终端里下载并运行 phi3 模型
```bash
ollama run phi3
```

## 11. 体验模型效果

模型有 2.4 G，等模型下载完毕后，就可以测试模型的效果了。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/aaa7cdf952582cecf9e952839ca2931ac181f07d.png">`

## 12. 也可以下载其他模型

https://ollama.com/library

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/12dc8bc847db26ccd831af9c8b6977a4321d413f.png">`

比如下载并运行 Gemma
```bash
ollama run gemma
```

<!-- ##{"timestamp":1718351611}## -->