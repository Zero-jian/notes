# notes
记录在项目开发中遇到的难点，总结解决方案！！

### 1. Vue的兼容性问题
* 引入babel-polyfill包做垫片，ES6方法转化成ES5方法（但不能转化node_modules里面的包，如果里面的插件使用了ES6，将无法转化从而兼容失败）
* 在webpack.config.js里面进行配置，babel-loader配置里面添加includes，将node_modules里面需要ES6转化的包填进去（这个时候基本上已经完成兼容了，但是会存在一个问题，打包出来的文件是使用use strict严格模式的，这时候在非严格模式下编写的代码可能会出现问题，这个时候就需要去除use strict严格模式）
* 自己手动编写loader，对打包出来的文件进行过滤，去除use strict模式（具体参考掘金收藏的文章）
* 通过上述步骤，可以完成ES6转化成ES5，使Vue兼容IE9以上浏览器


### 2. 配置环境变量
* 在node安装目录上全局安装cnpm => npm install cnpm -g --registry=https://r.npm.taobao.org（具体查看cnpm官网）
* 安装后日志会显示安装路径 => C:\Users\huguojian\AppData\Roaming\npm\cnpm
* 在环境变量Path配置 => C:\Users\huguojian\AppData\Roaming\npm
* npm代理淘宝镜像 => npm config set registry http://registry.npm.taobao.org/ 
* npm安装包提示格式出错，可能是因为复制的包文字格式不同


### 3. vscode插件配置
* 文件头部注释
* 代码格式化
* 代码静态化检测
* 路径跳转
* 项目文件夹图标


### 4. H5软键盘遮挡输入框以及ndroid和ios的弹起区别
* 思路就是监听软键盘弹起，手动调用window.scrollTo()滚动制定位置，记得使用延时setTimeout，Android和ios的监听方法有所区别
* android：软键盘会压缩页面，修改页面的高度，可以使用window.addEventListener('resize', function)去监听软键盘（把页面设置成制定高度即可避免压缩，却仍然会触发resize）
* ios：软键盘不会压缩页面，而是会把页面设置滚动，可以使用focus和blur监听软键盘


### 5. 创建分支并关联新的远程分支
* 查看全部分支 => git branch -a
* 创建新分支 => git checkout -b <name>
* 上传远程分支 => git push origin <name>
* 此时创建的分支存在本地和远程，需要把本地和远程分支关联起来 git branch --set-upstream-to=origin/<name>
* 查看分支关联 => git branch -vv


### 知识点总结
* axios取消请求 => 请求超时 或者 切换页面的时候使用
* vue-promised => vue加载内容的提示库 => 加载的时候可以显示loading图标


### 问题总结
* ios上overflow: hidden和position不能共用，否则overflow会失效
* v-html渲染的文本节点无法修改样式，可以通过单独的style里面编写样式

### 代码片段
* tbody内容溢出滚动：
`
tbody { 				
  display: block;
  height: 80px;
  overflow-y: scroll;
  overflow-x: hidden;
}
thead,tr {
  display: table;
  width: 100%;
  table-layout: fixed;
}
 /*关键设置：滚动条默认宽度是16px 将thead的宽度减16px*/
thead {
  width: calc(100% - 1em);
}
table thead th {
  background: #ccc;
}
`