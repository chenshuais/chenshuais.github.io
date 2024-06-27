# 前言 #

GitLab 是一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的web服务。
可通过网页进行访问公开的或者私人项目。它拥有与Github类似的功能，能够浏览源代码，管理缺陷和注释。不过GitLab可以免费创建自己的私密仓库，而Github创建私密仓库需要每个月支付7$。GitLab需要自己在服务器上搭建，所以拥有对用户的控制权，可以管理团队对仓库的访问，方便管理，适用于公司内部使用。现在就让我们来试试如何搭建自己的GitLab服务器。

先贴出参考学习链接：


> [https://www.douban.com/note/640641236/](https://www.douban.com/note/640641236/ "gitlab安装与使用10.0.3")

----------

# 正文 #

## 搭建前的准备

- 首先你要有一个操作系统为CentOS7.2的电脑或服务器，可以自己创建虚拟机或买个云服务器，这里就不教具体步骤了。

- 然后你需要一篇教程，比如这个^_^!

- 最后能看懂文字，并照着敲下来

----------


##  开始搭建

这里我使用的是阿里云服务器，1核2G内存。。。

### 1. 打开HTTP和SSH访问

进入系统后，输入以下命令，一条一条的运行，如果服务器配置垃圾的话，会比较慢，等会他就行，别着急，以为它坏了

#### 1.1 安装一些东西

屏幕上会输出一堆东西，最后出现 Complete! 就代表成功了

	sudo yum install -y curl policycoreutils-python openssh-server

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6c258472a11457f3acedeccbd030a4f25a7b1454.png">`

#### 1.2 开启SSH

这两条指令不会有什么东西输出，不要担心

	sudo systemctl enable sshd

	sudo systemctl start sshd

#### 1.3 开启HTTP

	sudo firewall-cmd --permanent --add-service=http

	sudo systemctl reload firewalld

如果运行第一条指令后出现 FirewallD is not running ，说明防火墙没有打开，运行一下

	systemctl start firewalld

来打开防火墙，之后再运行第一条指令，就会出现 success 

### 2. 安装GitLab

有的教程还会安装postfix来发送通知邮件，不过这得需要你有服务器的外部DNS名称，我试了3次都没设置成功，所有建议你别安装了，等后面再配置外部SMTP服务器，这个操作成功率比较高。

#### 2.1 添加GitLab包库 

这里屏幕上也会输出一堆东西，最后看到 The repository is setup! You can now install packages. 就说明成功了

	curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3fe34f3242b69f2323ddf74b04ca7a7db6d797d4.png">`

#### 2.2 安装GitLab

这里也会输出一堆东西，你会看到有下载进度，和安装进度，最后安装完成后屏幕上会输出 GitLab 的 logo

	sudo yum install -y gitlab-ee

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/83f507764f38f9055b1fb82dfa9293b2d62848f6.png">`

### 3. 配置并启动GitLab

如果服务器配置垃圾的话，这里会花费大量时间，可能会等半个小时左右吧，所以耐心等待，不要以为它坏了。最后输出 gitlab Reconfigured! 就代表成功了。

	sudo gitlab-ctl reconfigure

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/522ca6c8a1d496c571b0efa09170bc30b4edb3df.png">`

### 4. 可以使用啦

打开一个浏览器，输入你安装GitLab的服务器IP地址，就会打开一个网页。

第一次进入会让你设置密码

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b04b1372acef699aa81af559cf3b6c7058b7256e.png">`

设置完后会跳到登录界面

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f999d26b126936330cb590d85a2b1a2a99614f27.png">`

最后，终于进入 GitLab 了

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/af1c7fe298b44340ffeca285d35c26c8f3c8e90b.png">`

### 5. 修改GitLab里的项目展示地址
	
使用编辑器打开 gitlab.rb 文件，进行修改，输入以下命令

附上 vim 的使用方法：[https://www.cnblogs.com/emanlee/archive/2011/11/10/2243930.html](https://www.cnblogs.com/emanlee/archive/2011/11/10/2243930.html)

	vim /etc/gitlab/gitlab.rb

开始是不能进行编辑的，需要按 i ，屏幕左下角出现 INSERT ，然后就可以编辑了。

找到 external_url 'http://www.gitlab.com' ，改成你的地址就行了，也可以填IP地址

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d80a7f529f94142656a04b2727f52c9c39ad682f.png">`

保存退出，先按esc键，然后按 : ，最后输入 wq 

修改配置文件后，需要重启GitLab，输入以下命令，这里也会等很长时间

	gitlab-ctl reconfigure

	gitlab-ctl restart

----------

##  配置外部SMTP服务器，实现邮箱通知功能

需要有第三方邮箱服务账号，这里我用的是腾讯企业邮箱。如果你已经有账号了，就可以跳过这里。

### 1. 注册帐号

先按照步骤，注册帐号，链接： [注册腾讯企业邮箱帐号](https://exmail.qq.com/cgi-bin/bizmail_portal?t=bizmail_trial_signup&refer=index_top)

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/5c690bb84226ff8930e7fc140104b5ce763bba47.png">`


### 2. 添加域名

注册完之后，会让你设置一个域名，如果没有跳转，可以到 我的企业 -> 域名管理 -> 添加域名，进行设置

这里没有域名的，可以现买一个，有便宜的，不过你新买的域名需要实名认证，一般一天就完成认证了。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/ce534eb8fac7808dd1a3f84fe28a3cded8bd9147.png">`

然后会让你设置邮箱解析，按照它的步骤来就可以

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/7ecdeb655ddb0c23fd6ffe0eb228f5290b70476e.png">`

### 3. 添加成员用户

进入 成员与群组 -> 新增成员 -> 新建成员 ，添加一个用户。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/00ec58b37343ca775d7f8156da14b91fba3e0082.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/93e66c70d3bd20f67dc096fd152e1587051cc951.png">`

### 4. 设置管理员邮箱

进入 我的企业 -> 账号设置 ，找到管理员邮箱，进行添加，这里邮箱必须是成员邮箱

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/2afcc74c515eb3ec3087ec3aa4ae0f3283e4ec16.png">`

### 5. 获取客户端专用密码

先登录进管理员邮箱，就是上面添加的那个用户：[登录管理员邮箱](https://exmail.qq.com/cgi-bin/loginpage)

进去后找到设置，点击微信绑定，绑定后开启安全登录。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f5067219237cdf14f8d876b0fbcf3ed686241d0b.png">`

然后找到客户端专用密码，点击生成新密码按钮，记住得到的16位密码，一会要用到。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3549393052255d82e87efff2ff47b05a2a1e8a24.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/151cd7a3513d0b636a02b0b530915aafbc92ea14.png">`

### 6. 修改 GitLab 配置文件

使用编辑器打开 gitlab.rb 文件，进行修改，输入以下命令

	vim /etc/gitlab/gitlab.rb

修改保存的方法一样，找到如下内容

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/eb1298ad8f689392a58c96e93263e124541480a4.png">`

将打码的部分替换成自己的邮箱帐号和刚刚得到的16位密码

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/e87c36b1684b597d27970acf5de23ce25b8582a6.png">`

最后保存退出，先按esc键，然后按 : ，最后输入 wq 

重启GitLab，输入以下命令，继续等很长时间

	gitlab-ctl reconfigure

	gitlab-ctl restart


好了，重启成功后，就能使用邮件通知功能了。

----------

# 结言 #

如果想正常使用，推荐服务器配置是2核4G的，否则你的 CUP 会卡爆，打开网页能把你急死，还会时不时给你来个 502 服务器异常。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/325aa856924ce6d2a37d24453388cc2dd1236495.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/764ef6b0e459951703826358a6ca0fec247638f9.png">`

好了，关于 GitLab 的一些使用，下次再分享吧，但愿懒癌不会发作。`Gmeek-html<img src="https://blog-image.ian2018.cn/images/cba07c0042ae9355164cced9c1d662b1152cfc65.png">`


<!-- ##{"timestamp":1513250960}## -->