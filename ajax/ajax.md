# 原生 AJAX

## 1.1 AJAX 简介

- AJAX 全称为 Asynchronous Javascript And XML，就是异步的JS 和 XML。
- 通过 AJAX 可以在浏览器中向服务器发送异步请求，最大的优势：无刷新获取数据。
- AJAX 不是新的编程语言，不是新的一门独立的技术，而是一种使用现有标准的新方法

## 1.2 XML 简介

- XML 可扩展标记语言
- XML 被设计用来传输和存储数据。
- XML 和 HTML 类似，不同的是HTML 中都是预定义标签，XML中没有预定义标签，全都是自定义标签，用来表示一些数据。

## 1.3 AJAX 的工作原理

- Ajax 的工作原理相当于在用户和服务器之间加了一个中间层（Ajax引擎），使用户操作与服务器响应异步化。

## 1.4 AJAX 的特点

### 1.4.1 AJAX 的优点

- 可以无需刷新页面而与服务器端进行通信。
- 允许你根据用户事件来更新部分页面内容。

### 1.4.2 AJAX 的缺点

- 没有浏览历史，不能回退
- 存在跨域问题
- SEO 不友好

## 1.5 语法

- ```
  // 实例化一个XMLHttpRequest对象
  let xhr = new XMLHttpRequest();
  
  // 绑定一个事件监听，名称为：onreadystatechange
  xhr.onreadystatechenge = function () {
  	/*
  	* 在xhr内部有5种状态：
  	* - 0：在xhr被实例化出来，状态就是0 ，即：初始化状态
  	* - 1：请求还没有发出去，即：send方法还没有被调用，依然修改请求头
  	* - 2：请求以及发出去了，即：send方法已经被调用了，不能再修改请求头，响应首
  	* 行和响应头已经回来了，可以获取到响应首行和响应头
  	* - 3：数据已经回来了（但是数据可能不完整，如果数据小，会在此阶段直接接收完	* 毕，数据大有待于进一步接收）
  	* - 4：数据完全回来了
  	*/
  	if(xhr.readyState === 4 && xhr.status == 200) {
  		console.log(xhr.response)
  	}
  }
  
  
  /* POST请求：
   * // 如果是POST请求要加上这行请求头
   *  xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');
   * // 指定发请求的：方式、地址、参数
   * xhr.open('GET','http://localhost:8080/login')
   * xhr.send('name=joker&age=18')
   */
   /* GET请求：
    * // 指定发请求的：方式、地址、参数
    * // GET请求：xhr.open('GET','http://localhost:8080/login? name=joker&age=18')
    * xhr.send()
    */
  
  ```

# 跨域问题总结

## 1. 为什么会有跨域这个问题？

- 原因是浏览器为了安全，而采用的同源策略（Same origin policy）

## 2. 什么是同源策略？

1. 同源策略是由netscape提出的一个著名的安全策略，现在所有支持JavaScript的浏览器都会使用这个策略
2. web 是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。
3. 所谓同源是指：协议，域名（IP），端口必须完全相同

## 3. 非同源受到哪些限制？

1. Cookie不能读取；
2. DOM无法获取；
3. Ajax请求不能发送

## 关于jsonp解决跨域的说明

1. 原理：利用了标签的src属性发送了get请求（不受同源策略的限制）
2. 套路：
   - 创建script节点，指定src，利用标签把请求发出去
   - 定义好一个处理数据的函数
   - 把数据处理函数的名称传递好后端
   - 后端返回符合js函数调用语法的字符串
3. 局限性
   - 只能解决get请求跨域问题
   - 必须需要后端人员配合

- 注意：服务端可以通过设置响应头 response.set('Access-Control-Allow-Origin','允许访问的地址'); 