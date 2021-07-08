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


### 6. git pull拉取文件冲突
* git stash => 暂存本地代码
* git pull =>  拉取线上代码
* git stash pop => 还原暂存内容
* 代码自动合并，通过vsCode手动取舍冲突代码

### 7. 响应式和自适应的区别
* 响应式 => 页面在不同的屏幕尺寸下,都有良好的布局和内容表现，适配多种不同尺寸的屏幕，,例如pc端显示的页面转到移动端就叫响应式
* 自适应 => 页面的设计与设计稿的设计比例一致


### 8. 项目图片处理
* 采用两倍图，适应高清屏
* 懒加载，加快页面载入速度
* 上线前压缩图片
* 懒加载图片加载时页面高度会发生变化，这时候可以预先计算并设置图片高度，避免页面高度忽变


### 9. webpack知识点
* entry => 编译入口文件、公共文件(common.js)


### 10. 引入字体知识点
* 优先推荐使用系统自带字体
* pc端使用系统自带字体，若该系统不存在该字体则使用默认字体，移动端不存在该情况
例如：font-family：'微软雅黑'
若pc端浏览器不存在微软雅黑字体，则会默认使用宋体
* 使用自定义字体需要引入多种格式的字体(.eot/.woff/.woff2/.ttf)，已应对兼容性问题
* 从服务器引用自定义字体可能会存在版权问题
* 引用字体过大，可能会造成页面加载过慢，会看到页面字体的变化，可使用遮罩层掩盖
* 可以引用多种字体，然后针对性使用字体
* 可以使用字蛛(font-spider)可以只导入使用的字体，从而压缩字体大小
* 方案一：简易方案 => 引入字体会导致页面加载过慢，可以在首先在body上设置font-family默认字体，在从script里动态设置引入的页面字体，增加了页面的加载速度。
缺点是页面会出现字体的变化
* 方案二：最优方案 => 检测该浏览器是否支持指定系统字体，若不支持则导入自定义指定字体。
判断浏览器是否支持该字体 => https://www.zhangxinxu.com/wordpress/2018/02/js-detect-suppot-font-family/
动态创建link引入css文件


### 11. Vue页面执行顺序
* 页面 => beforeCreate -> state(data, computed, methods) -> created -> beforeMounte -> mounted -> watch
* 页面加载 => watch(immediate：true) -> computed -> mounted -> watch(默认,在页面加载后才调用)
* 父子组件 => beforeCreate(父) -> created(父) -> beforeMounte(父) -> beforeCreate(子) -> created(子) -> beforeMounte(子) -> mounted(子) -> created(父)


### 12. scoped和module的区别
* scoped => 在class上添加hash标识属性，在vue组件里的每个元素都拥有同一个hash标识属性，无法避免权重和类名重复的问题
* module => 生成独一无二的class，为所有类名重新生成，有效避免权重和类名重复的问题
* 如果子组件有一个类名在父组件上已经定义过，那么使用scoped，会泄露样式到子组件中，但module可以有效避免这个问题
* 还有一些情况，父组件需要修改子组件的样式，使用scoped可以使用/deep/和>>>来实现，但这就失去了组件样式封装的意义了，并且子组件的所有相同样式都会被影响到，
使用module可以通过$style对象获取样式，然后通过props传递，这样能有效保持组件样式封装，并且成功修改样式


### 知识点总结
* axios取消请求 => 请求超时 或者 切换页面的时候使用
* vue-promised => vue加载内容的提示库 => 加载的时候可以显示loading图标
* 自适应 => 标准设计图以375px为标准，使用rem自适应，把body设置成100px，转换rem方便计算


### 问题总结
* ios上overflow: hidden和position不能共用，否则overflow会失效
* v-html渲染的文本节点无法修改样式，可以通过单独的style里面编写样式

### 代码片段
* tbody内容溢出滚动：

`
table, thead, tbody {
  display: block;
}
thead, tbody tr {
  display: table;
  width: 100%;
  table-layout: fixed;
}
tbody {
  height: 100px;
  overflow-y: scroll;
  overflow-x: hidden;
}
thead {
  width: calc(100% - 10px)
}
`