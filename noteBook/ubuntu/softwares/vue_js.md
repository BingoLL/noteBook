### ubuntu 安装vue

需要安装nodejs 
我的电脑已经安装nodejs，但是我需要把它升级到最新稳定版 
node有一个模块叫n，是专门用来管理Node.js的版本的。

首先 安装n模块

`npm install-g n`

第二步，升级

`n stable`

安装vue

`sudo npm install -g vue`

安装vue-cli 命令行工具

`npm install -g vue-cli`

创建一个基于 webpack 模板的新项目

`sudo vue init webpack my-project`

```
sudo cd my-project
sudo  npm install
sudo npm run dev
```


### 解决npm update的权限问题

```
sudo chown -R bingo ~/.npm
sudo chown -R bingo ~/node_modules
```

### Vue中正确使用jQuery的方法

1. NPM 安装 jQuery，项目根目录下运行以下代码
```
npm install jquery --save
```
2. webpack配置
在项目根目录下的build目录下找到webpack.base.conf.js文件，在开头使用以下代码引入webpack，因为该文件默认没有引用。
```
var webpack = require('webpack')
```
然后在module.exports中添加一段代码，
```
// 原有代码
resolve: {
extensions: ['.js', '.vue', '.json'],
            alias: {
                'vue$': 'vue/dist/vue.esm.js',
                '@': resolve('src')
            }
},
    // 添加代码
plugins: [
    new webpack.ProvidePlugin({
            $: "jquery",
            jQuery: "jquery",
            jquery: "jquery",
            "window.jQuery": "jquery"
            })
],
    // 原有代码
    module: {
rules: [
           // ......
]
    }
```
然后许多其他解决办法到此就说在main.js里导入就可以了，然而题主照着做了。
main.js里导入jQuery
```
import 'jquery'
```
在Vue组件里使用 $ or jQuery 写了操作dom的代码
接着启动项目
```
npm run dev
```
但是编译却报错了：

http://eslint.org/docs/rules/no-undef '$' is not defined or
http://eslint.org/docs/rules/no-undef 'jQuery' is not defined
咋回事呢？？？
3.eslint 检查
机智的朋友肯定想到跟eslint有关，没错，这时候需要做的下一步就是要修改根目录下.eslintrc.js文件了，在改文件的module.exports中，为env添加一个键值对 jquery: true 就可以了，也就是：
```
env: {
    // 原有
browser: true,
             // 添加
             jquery: true
}
```
再次 npm run dev ，OK了，没报错，赶紧去组件里试一下吧，console.log($('选择器')) ，你会发现成功使用jQuery获取到了DOM。
