---
title: 关于puppeteer
date: 2018-01-20 15:23:24
tags: puppeteer
categories: Node
---

### 介绍一下Puppeteer

>Puppeteer是一个node库，他提供了一组用来操纵Chrome的API（默认headless也就是无UI的chrome，也可以配置为有UI）
>
>有点类似于PhantomJS，但Puppeteer是Chrome官方团队进行维护的，前景更好。
>
>使用Puppeteer，相当于同时具有Linux和Chrome的能力，应用场景会非常多。就爬虫领域来说，远比一般的爬虫工具功能更丰富，性能分析、自动化测试也不在话下，今天先探讨爬虫相关

### [详情请看Puppeteer官方文档](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#class-browser)

阅读以下内容，可以实现爬取[片刻网](http://pianke.me)的首页数据。

#### Puppeteer 几个重要功能
* 可以抓取网站内容
* 可以自动化表单提交、搜索网络内容、UI测试等
* 获取网页生成的PDF、图片、视频等资源

接下来的操作基于 Node 环境

`mkdir test && cd test`

`npm init`

`npm install puppeteer`

<!--上述命令墙内用户可能会有问题，建议科学上网-->

#### Puppeteer 基本用法

1.使用`puppeteer.launch()`运行puppeteer，他会return一个promise，使用then方法获取browser实例，[Browser API](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#class-browser).

2.拿到browser实例后，通过`browser.newPage()`方法，可以得到一个page实例，[Page API](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#class-page).

3.使用`page.goto()`方法，[跳转至ES6标准入门](http://es6.ruanyifeng.com/#README).

4.在`page.evaluate()`方法中注册回调函数，并分析dom结构，从下图可以进行详细分析，并通过`document.querySelectorAll('ol li a')`拿到想要获取的内容.

#### 具体代码

[点我查看详细代码](https://github.com/easy261925/Puppeteer)

或 `git clone git@github.com:easy261925/Puppeteer.git`

#### 说一下我遇到坑吧

1. 在配置环境的时候由于网络的原因，在安装 Puppetee 的时候可能会无法安装,[ node_modules 百度云下载地址 ](https://pan.baidu.com/s/1dIMKjk) 密码: bxhy. 不需要 `npm i`了, 下载完后直接使用`node getData.js`即可.
2. 代码的实现过程, 在看过网上很多关于 Puppeteer 技术文章和同学大力帮助之后，也算把想要的结果得到了，但是关于 async Promise await 等函数以及 ES6 Array
的新用法还不是特别清楚， 本人打算学习[ 阮一峰老师的ES6 ](http://es6.ruanyifeng.com/#README)之后, 在来详细补充其原理.
3. 爬取到的数据是通过 class 名，找到里面元素的 src/style/innerText，但是获取到的数据格式为:` img:['1.jpg','2.jpg'];title:[ 'AAAAAAA','BBBBBBB'] ` 为了使用方便，我们对数据做了一些调整，让它变为`[{item1:['1.jpg','AAAAAAA']},{item1:['2.jpg','BBBBBBB']}]`,这样我们在铺到页面上时使用更加方便了.( 经历此过程也体验了一回写 API 接口的感觉 全是眼泪啊~~~ )


### 结语
由于能力有限,存在的问题还有很多，请看到此文的人不吝赐教，感激不尽。

特别鸣谢:[QC-L老师](https://github.com/QC-L)，[marryyan ](https://github.com/marryyan)的帮助.

### 未完待续。。。
























