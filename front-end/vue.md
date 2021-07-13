# vue前端三大框架
## 特点：
1. 只关注视图层
2. 用户无需再关注DOM元素
## 框架和库的区别：
1. 框架对项目的入侵性质较大，更换框架基本意味着重做
2. 库对项目的入侵性质较小，一个库不行可以调用另一个库
## MVC和MVVM的关系
1. MVC是后端分层开发概念，module,vision,controller
2. MVVM是前端视图层的概念，
## vue和MVVM的对应关系
## 一些指令
1. v-cloak 阻止闪烁
2. v-text展示为文本
3. v-html展示为HTML
4. v-model实现双向数据绑定，只能用在表单元素中，如text,radio,email,checkbox
5. v-for必须使用key来绑定
6. v-if每次都会重新创建或删除元素
7. v-show不会，只是展示元素的display:none属性
## 事件修饰符
1. stop阻止冒泡，即只发生添加.stop的事件
2. prevent阻止默认事件
3. capture在添加事件侦听器时采用捕获模式
4. self只在本身触发时触发回调
5. once只触发一次
## class
1. class的数组表达可以使用三元表达式
## axios库
1. axios必须导入
2. get或者post方法发送对应请求
3. then的回调函数在请求成功或失败时触发
4. 回调函数的形参可以获取响应内容或者错误信息
5. 回调函数中this已经改变，无法访问到data的数据
6. 和本地应用最大的区别是改变了数据来源