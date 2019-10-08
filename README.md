# Introduction

## 项目是用gitbook启动可在本地浏览器查看
*Figure 1*

GitBook 是一个基于 Node.js 的命令行工具，下载安装 Node.js，安装完成之后，你可以使用下面的命令来检验是否安装成功。
```
$ node -v
v7.7.1
```
*Figure 2*

安装 GitBook
```
$ npm install gitbook-cli -g
```
安装完成之后，你可以使用下面的命令来检验是否安装成功。

```
$ gitbook -V
CLI version: 2.3.2
GitBook version: 3.2.3
```
*Figure 3*

GitBook 准备工作做好之后，我们进入一个你要写书的目录，输入如下命令
```
$ gitbook init
warn: no summary file in this book
info: create README.md
info: create SUMMARY.md
info: initialization is finished
```
可以看到他会创建 README.md 和 SUMMARY.md 这两个文件，README.md 应该不陌生，就是说明文档，而 SUMMARY.md 其实就是书的章节目录。

*Figure 4*

接下来，我们输入 $ gitbook serve 命令，然后在浏览器地址栏中输入 http://localhost:4000 便可预览书籍。

*Figure 5*

book.json
该文件主要用来存放配置信息，我先放出我的配置文件
