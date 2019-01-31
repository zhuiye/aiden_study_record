# centos7搭建node.js 环境

## 参考链接

基本搭建:

https://help.aliyun.com/document_detail/50775.html?spm=5176.doc25429.6.644.3D2aMv

防火墙开放端口:

https://blog.csdn.net/yyycheng/article/details/79753032

## ssh 链接 vps

`ssh   root@192.179.85.187`

NVM多版本安装(我采取的是这个)

1.安装git 并把mvn clone 下来**

```
yum install git
git clone https://github.com/cnpm/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
```

2.激活NVM

```
echo ". ~/.nvm/nvm.sh" >> /etc/profile
source /etc/profile
```

3.列出Node.js 的所有版本

`nvm list-remote`

4.安装多个Node.js 的所有版本

```
nvm install v10.14.2
nvm install v7.4.0
```

5.运行 nvm ls 查看已安装的node.js版本

6.运行 `nvm use v7.4.0` 切换Node.js版本至v7.4.0

## 部署测试项目

1.新建项目文件example.js

```
cd ~
touch example.js
```

2.使用vim编辑器打开项目文件example.js。

```
yum install vim
vim example.js
```

输入 `i`，进入编辑模式，将以下项目文件内容粘贴到文件中。使用 `Esc` 按钮，退出编辑模式，输入 `:wq`，回车，保存文件内容并退出。

**项目文件内容：**

```javascript
const http = require('http');
const hostname = '0.0.0.0';
const port = 3000;
const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello World\n');
});
server.listen(port, hostname, () => {
	console.log(`Server running at http://${hostname}:${port}/`);
});
```

3.运行项目

`node ~/example.js`

4.使用命令查看项目端口是否存在。

 `yum install net-tools`   因为centos7不带

  `netstat -tpln`

 `netstat -tunpl` 查看开放的服务端口

## 防火墙开放端口

`firewall-cmd --zone=public --permanent --add-port=3000/tcp`  

`firewall-cmd --reload ` 重启一下

## Node.js在后台运行

**1.安装**

`sudo npm install forever -g #记得加-g，forever要求安装到全局环境下`

**2.启动**

`forever start app.js`

**forever文档**

 https://github.com/nodejitsu/forever