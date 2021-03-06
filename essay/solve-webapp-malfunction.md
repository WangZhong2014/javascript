# 解决 WebApp 运行异常的问题

## 现象描述

今天帮胡同学解决了 WebApp 运行异常的问题，具体是个什么现象呢？就是在终端终止了 WebApp 之后，在浏览器访问 WebApp 的 URL 的话，可以正常显示页面，并且在终端会看到又有网络请求了，说明 WebApp 又被启动起来了。这是什么灵异现象？

## 检查项目库

问了问胡同学，也没有配置什么 pm2，只是简单地初始化了一下项目。自己看了看 `package.json`，的确也只是最基本的几个库，没有 pm2 这种监控服务的库。

## 检查执行指令

胡同学最开始在微信上说是用 `cnpm start` 来启动的，然后就会有上面这种情况。自己依稀记得这两天刚好在网上看到过一篇什么文章，里面提到过一句，说用 cnpm 运行服务有可能会有问题（最后问题解决之后发现其实跟 cnpm 无关）。于是尝试用 `npm start`，用 `node ./bin/www` 来启动项目，还是有这个问题，说明原因不在指令上。

## 升级 Node.js 和 npm

后来又想，会不会是胡同学用的 Node.js 和 npm 下载不完整，所以用得有问题？看了一下胡同学的 nvm，是最新的 1.1.6，又看了看 nvm 的配置，用的也是淘宝镜像作为下载源。于是执行 `nvm install latest` 下载安装最新版的 Node.js 和 npm，并且执行 `nvm use 9.8.0`，使用最新版的程序。然后再启动项目，发现还是有这个问题。

## 更换项目路径

无意中看到胡同学项目所在路径有中文名，并且在 C 盘，那么会不会是对中文名支持不好（其实不是这个原因），或者是当前路径权限不够呢？（也不是这个原因，权限不够应该就无法新建项目）

先在 D 盘试试，新建了一个英文文件夹 `code`，在下面新建 `express` 文件夹，全局安装 `express-generator` 之后，在这个 `express` 文件夹中初始化 express 项目并安装依赖，然后再运行项目。再打开 360 浏览器访问 `localhost:3000`，还是打不开，什么鬼？

因为自己向来对国产浏览器不放心，刚好看到胡同学电脑上有 Chrome 浏览器，于是在 Chrome 浏览器中访问项目地址，可以打开了！哈哈。然后又在 360 中访问，这回也可以访问了，你妹的！

嗯，这样一来排除了两个可能的原因，既不是浏览器的原因，也不是 Node.js 这一套开发环境的原因。

然后接着在胡同学位于 C 盘目录下的项目所在文件夹的隔壁，再新建一个 `express` 文件夹，照着上面的流程初始化并运行项目，这个时候在浏览器中依然可以正常访问。

啊哈，看来也跟 C 盘或者中文路径什么的没关系，这个时候胡同学又想起来当时初始化有问题的那个项目时，终端是有报错的。那么项目运行异常的原因很可能就在这里。

问了一下胡同学，有问题的那个项目也没写什么东西，就直接删掉好了。

## 补充

一点小建议：如果在安装库的时候，终端中有错误提示，可以试着删除库并重新安装。因为有时候网络不稳定，下载的文件会不完整，但是 nvm 和 npm 本身并没有文件的完整性检查机制，所以就会出现各种状况。去年在 JS 入门课的时候也遇到过好几次这个问题，有同学用 nvm 或者 npm 进行安装，安装过程有问题，只下载过来了部分文件，结果在写代码的时候就各种莫名其妙的事情。

再来一点小建议：做程序开发，最好尽量用原汁原味的 Chrome，而不是用哪些不知道改了什么鬼地方的 360、QQ、搜狗，这样也能避免一些奇怪的问题。
