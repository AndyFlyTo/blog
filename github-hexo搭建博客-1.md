---
title: github+hexo搭建博客
date: 2016-09-17 12:33:17
tags:
categories: hexo 
---



刚利用github+hexo搭建了这个博客，所以趁热打铁记录一下，开始我的第一篇博客（然而各种渣，并不知道怎么写，边写边学，见谅～）
看网上各位大神介绍的太简短了，对于我这种技术渣真是废了好大劲，所以写下我的安装配置过程。
由于我是在Ubuntu下搭建的，这里只介绍Ubuntu下。
下面开始准备搭建博客。

<!--more-->


![图片][1]


## **安装git** ##
用于把本地的博客内容提交到github上去，直接终端下下载即可
```
sudo apt-get install git
```
检验一下是否安装成功可以输入git --version，出现版本号即表示成功
## **安装配置Node.js** 
###  安装 
为什么要安装它？因为hexo是一个基于node.js的静态网页框架，所以必须安装了node.js,在其环境下才能使用hexo生成静态的网页。
安装建议直接去[node官网][2]download页面下载（如果使用包管理器一般版本比较老），这里有源码下载`Source Code`还有各种二进制压缩包，第一种手动编辑即是部署过程，感觉貌似比较复杂，坑比较多，所以直接下的.tar.gz包，
cd到想安装的位置解压安装

	tar zxvf <文件名>.tar.gz

解压后bin文件中有npm和node了，说明下载的没有问题，但是此时只能在此目录下使用npm命令，所以接下来需要配置环境变量。
```
cd bin
ls
node npm 
./node -v
v4.5.0(版本号)
```

PS：如果是下载的源码，看网上这样配置,可以试一下
```
  tar xvf node-v0.10.28.tar.gz 
 cd node-v0.10.28 
 ./configure 
 make 
 cp /usr/local/bin/node /usr/sbin/ 
```

###  **配置**
把安装的路径配置到**环境变量**中，为什么要配置环境变量，是因为我们只在这个目录下存在有node，如果我们想在任意目录下使用，则需要将其添加到环境变量中，让电脑从其中寻找这个路径。
pwd复制下node所在的路径，编辑`/etc/profile`或者`/etc/bash.bashrc`或者`.bashrc`(均为shell配置文件,/etc/bash.bashrc是在全局上定制shell，.bashrc位于用户主目录下，一般修改这个)在文件最后添加
**`PATH=$PATH:/home/chenchen/download/node-v4.5.0-linux-x64/bin`**将我的路径改成自己的bin路径
修改好了保存退出（如果使用vi打开，点击i进入编辑模式，退出时按Esc，然后输入：wq)
再使用source保存，使新改动内容立即临时生效，而不用重新登录,重新启动一下配置。（shell脚本文件，在用户登录后自动执行）
```
source /etc/profile
```
此时在任意路径下输入node -v 如果出现版本号及配置成功
这里要注意如果你的终端使用的是`oh-my-zsh`，则是在`~/.zsh`这个配置文件中配置Path（这里涉及到配置文件的覆盖问题，如果使用zsh则 `~/.zsh`则会覆盖一些系统级别的shell变量如PATH）我就是在zsh下，然后一直修改的是/etc/profile文件中的PATH，然后导致一直没有成功。

可以使用`echo $PATH` 打印当前path值。Linux下环境变量里是以冒号作为路径分隔符的。例如我的刚开始还未添加时是
	
	/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

添加后

	/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/chenchen/download/node-v4.5.0-linux-x64/bin

PS：刚开始配的时候看到有介绍软链接方式，但是不知道为什么没有成功，`ln -s`设置全局，任何用户都可以，不建议
> 软链接的意思是为一个文件在另一个目录下建立同步的链接,当需要在不同目录下用到相同文件时，不需要在每个目录下都放置这个文件，在一个目录下放置，只需要通过软链接的方式，即可使用，且不占用磁盘空间。
```
ln -s <解压路径>/bin/node /usr/local/bin/node
ln -s <解压路径>/bin/npm /usr/local/bin/npm
```
（此时whereis node 
可以看到node存在与上面两个路径中，但是这个配置的是全局的，既是任何用户都可以）
ps：在windows下比较简单，下载安装只需保持默认设置及配置成功

## **安装hexo**
接下来，终于可以安装hexo了。
首先，先了解一个什么是hexo，hexo是一个基于node.js的静态网页生成器，即是一个博客框架。详细了解请见[hexo][3]
cd到一个目录下（这里我用Hexo），用于存放hexo的各种文件，在终端下下载hexo-cli和hexo
```
cd hexo/
sudo npm install -g hexo-cli
```

如果上面配置node.js不成功,此时执行npm命令会出现`npm command not found  `，则需要再次配置环境变量出现了什么问题。
然后
```
hexo init
```
hexo即会在该文件夹下建立网站所需要的文件，然后会提示
```
npm install
```
按照提示安装依赖包，即会在此目录下安装node_modules,然后到此，本地博客已经搭建成功。
看一下吧。输入：
```
hexo generate
hexo server
```
hexo generate生成静态网页，hexo server本地预览博客，提示running at http://localhost:4000/ 然后到浏览器输入http://localhost:4000，可以看到Hello World的欢迎博客，但是现在还只能在本地看到，所以接下来要做的就是把它部署到Github上去。

如果说执行找不到hexo server命令，则是因server在hexo3.0后被独立出来了，只要下载就好了
> npm install hexo-server --save

如果出现什么问题，无法打开，提示不是running at http://Localhost：4000/，输入hexo server --debug看看是不是端口号已被占用，hexo s -p <新的端口号>来修改端口号。再在浏览器输入即可。

## **部署到github**
### 创建github帐号

访问地址[github官网][4]，sign up 输入邮箱，用户名，密码，进入之后，点击右上方+号，点击`New repository`创建新的仓库，然后填写Repository name **注意这个名字要和你的用户名相同，然后命名格式<用户名>.github.com**
### 配置hexo，将hexo与github关联
刚才Hexo文件夹下有一个_config.yml，这是hexo的全局配置文件，包含很多配置内容，即是在这里修改来关联github
打开_config.yml，在文件最后可以看到
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo:  https://github.com/AndyFlyTo/AndyFlyTo.github.io.git
  branch: master
```
按照我写的，第二项换成自己的帐号名，即关联了自己的github。

**注意**
 -  每一项冒号后有一个空格 
 -  此时暂时用`https`协议的仓库地址来写，且后面还有.git
 
hexo3.0以后deployer-git与hexo是分开的，所以要去下载。
```
sudo npm install hexo-deployer-git --save (下载安装deloyer-git)
hexo deploy （部署）
```
至此简单的部署已经完成
现在已经可以输入`http://<your_name>.github.io`看到你的博客了。
### 写博客
可以去你放置hexo的各种文件的目录下，可以看到很多文件，其中source文件夹_posts存放的是你的博客，所以cd到当前文件位置输入
```
hexo new "title" 
INFO  Created: ~/blog/source/_posts/title.md
```
hexo new "title"在当前目录下创建一个title.md，然后就可以打开编辑了。编辑完成后可以使用hexo server查看预览。如果要上传服务器，输入以下命令
```
hexo c                           清除缓存hexo clean
hexo g                           生成静态网页 generate
hexo d                           部署到服务器 deploy
```

过程中没有显示错误出现即上传成功，输入自己的博客地址看看吧。

更多hexo命令可以去[这里][5]查看
## **配置ssh**
进行到上面你会发现`hexo d`后会让你输入github的帐号和密码
```
Username for 'https://github.com':
Password for 'https://<your_name>@github.com':
```
每次都要输入比较烦，所以这里配置ssh，它可以让你每次不用输入帐号密码。
先设置你git的帐号邮箱，作为全局使用。
终端下输入
```
git config -global user.name <在此输入你想要设置的名字>
git config -global user.email <邮箱地址>
```
此存在与根目录下的.gitconfig文件中
###  申请ssh密钥
可以查看[官方教程][6]
首先，检查电脑中是否已经存在密钥，
``` 
ls -al ~/.ssh
```
当然还可以重新申请ssh密钥
```
ssh-keygen -t rsa -C "your_email"

Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

这个地方是第四行是生成一个文件来存放生成的密钥，如果回车即使用它默认的文件名id_rsa.pub，否则也可以自定义文件名。然后要求你输入密码，这个密码是防止别人向你的项目里提交内容，但是你自己提交的时候也要输入，所以直接回车就好。
然后当看到一堆图形时，![此处输入图片的描述][7]说明ssh密钥生成成功。
### 添加到github
如果上面使用的默认的文件名，此时即打开id_rsa.pub这个是你的公钥文件，id_rsa是你的私钥文件。这里需要id_rsa.pub,将其中全部复制。然后到自己的github里的SSH keys 然后点击new ssh keys ，这里有title描述，还有key即将复制的密钥粘贴到这里。然后点击Add SSH Key，github会让你输入帐号密码作为验证。
特别**重要**的，使用以下命令把专用密钥添加到ssh-agent的高速缓存中
**`ssh-add ~/.ssh/id_rsa`**
(ssh-add -l查看有过的密钥)

这是我踩过的坑
**验证** SSH密钥是否添加成功，链接
> ssh -T git@github.com

然后可能会提示如下信息
```
The authenticity of host 'github.com (192.30.252.1)' can't be established.
RSA key fingerprint is nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)?
```
输入yes即可，然后下面信息
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```
即表示成功了。如果出现`access denied`可能是密钥没有添加成功，或者像我一样没有ssh-add 。
然后现在试验一下是否还需要帐号密码，hexo n "title"—> hexo c —> hexo g —>hexo d发现还是需要密码。（哈哈哈哈），因为我们刚才在hexo的_config.yml的repo远程仓库使用的是https协议，这时因为配置的是ssh，所以我们更改为ssh协议的远程仓库
> git@github.com:<your_name>/<your_name>.github.io.git

这时，再次试验发现已经不用输入密码，到此，一个完整的博客已搭建完成。

## **更改主题**
hexo默认的主题实在是太丑了，而且不多样，但是我们可以通过配置来更改主题，样式等等。
知乎上有一个回答[主题][8]，这里面有很多主题可以选择，可以选择自己喜欢的


**安装插件**
安装 npm install 插件名 --save
卸载 npm uninstall 插件
更新 npm update




推荐[http://www.jianshu.com/p/df3edc4286d2][9]
[https://segmentfault.com/a/1190000004316787][10]


  [1]: /home/chenchen/%E5%9B%BE%E7%89%87/2016-09-15%2015-50-11%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png
  [2]: https://nodejs.org/en/download/
  [3]: http://hexo.io
  [4]: https://github.com/
  [5]: https://hexo.io/zh-cn/docs/commands.html
  [6]: https://help.github.com/articles/generating-an-ssh-key/
  [7]: dfgh
  [8]: https://www.zhihu.com/question/24422335/answer/46357100
  [9]: http://www.jianshu.com/p/df3edc4286d2
  [10]: https://segmentfault.com/a/1190000004316787
