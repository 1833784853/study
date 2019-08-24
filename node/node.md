#	Node

---

##  1.  特点

- 异步非阻塞 I/O 
- 适用于I/O密集型的应用
- 事件循环机制
- 单线程
- 跨平台

## 2. 不足

- 单线程，处理不好CPU密集型任务
- 回调函数嵌套太多，太深（回调地狱）

----

# Buffer 

## 1. Buffer是什么？

1. 它是一个类似于数组的对象，用于存储数据（存储的是二进制数据）。

 	2. Buffer的效率很高，存储和读取很快，直接对计算机的内存进行操作。
 	3. Buffer的大小一旦确定了，不可修改
 	4. 每个元素占用内存的大小为1字节。
 	5. Buffer是node中的非常核心的模块，无需下载，无需引入即可使用 

## 2. 创建Buffer对象

 1. 将一个字符串存入Buffer中,并创建

    > let str = 'hello';
    >
    > let buf = Buffer.from(str);

 2. alloc这种方式去创建Buffer实例 -- 效率一般，重新拿一块新的内存

    > let buf = Buffer.alloc(10);

 3. allocUnsafe这种方式去创建Buffer实例 -- 效率高 但是存在安全问题，是从内存中拿一块，但是并不会把数据清干净

    > let buf = Buffer.allocUnsafe(10);

 4. 使用new关键字创建 ---效率低

    > let buf = new Buffer(10);

---

# 语法与函数

## 1. 写入文件

### 1.1 简单写入

- **不足 -- 简单写入是一次性把所有的数据写入到文件中，但是如果文件太大,内存装不下就会溢出，就会造成文件缺失适合较小文件**

- **fs.writeFile(file, data[, options], callback)**
  - file --文件路径+文件名
  - data --要写入的数据
  - option -- 配置参数（可选参数）
    - flag -- 打开文件进行的操作 默认是 'w'
      - w 是写入
      - a 是追加
    - mode -- 文件权限限制，默认 0o666
      - 0o111 文件可被执行
      - 0o222 文件可被写入
      - 0o444 文件可被读取
    - encoding  -- 默认编码 utf8
  - callback 回调函数
    - err 错误对象

### 1.2 可写流

- **把较大的文件通过运输的方式写入文件**
- **开启流一定要绑定监听**
- **fs.createWriteStream(path[,options]);**
  
  - path -- 写入的文件路径+文件名
  - option -- 配置参数（可选参数）
    - flags -- 打开文件进行的操作 默认是 'w'
      - w 是写入
      - a 是追加
    - mode -- 文件权限限制，默认 0o666
      - 0o111 文件可被执行
      - 0o222 文件可被写入
      - 0o444 文件可被读取
    - encoding  -- 默认编码 utf8
    - fd -- 文件唯一标识
    - autoClose -- 自动关闭，当数据操作完毕，自动关闭文件，默认true
  - start -- 文件写入的起始位置
  
- ```node
  let ws = fs.createWriteStream('./demo.txt');
  ws.on('open',() => {
  	console.log('流开启了');
  })
  ws.on('close',() => {
  	console.log('流关闭了')
  })
  // 写入数据
  ws.write('你好');
  // 关闭流，注意如果是8~9的版本会出现问题建议用ws.end()
  ws.close()
  ```

  

## 2. 读取文件

### 1.1 简单读取

- **fs.readFile(file[, options], callback)**
  - file --文件路径+文件名
  - option -- 配置参数（可选参数）
    - flag -- 打开文件进行的操作 默认是 'r'
      - r 读文件
      - r+ 读写文件
    - encoding  -- 默认编码 utf8
  - callback 回调函数
    - err 错误对象
    - data 数据对象Buffer

### 1.2 可读流

- **fs.createReadStream(path)**

  - 对于可读流来说 当没有可读的数据后，会自动关闭

  - path -- 读取文件的路径

- ```
  let rs = fs.createReadStream(./demo.mp3);
  rs.on('open',() => {
  	console.log('可读流开启了')
  })
  rs.on('close',()=> {
  	console.log('可读流关闭了')
  })
  rs.on('data',(data) => {
  	
  })
  ```

# Mongoose 的使用

## 1.  简介

- Mongoose 是一个对象文档模型（ODM）库，它对Node 原生的 MongoDB 模块进行了进一步的优化封装，并提供了更好的功能。

## 2. 优势

- 可以为文档创建一个模式结构（Schema）
- 可以对模型中的对象、文档进行验证
- 数据可以通过类型转换转换为对象模型
- 可以使用中间件来应用业务逻辑挂钩
- 比Node 原生的 MongoDB 驱动更容易

## 3. 语法

### 3.1 Create：

> 模型对象.create(文档对象，回调函数)
>
> 模型对象.create(文档对象) //返回值是一个promise对象

### 3.2 Read

> 模型对象.find(查询条件[, 投影]) 不管有没有数据都返回一个数组
>
> 模型对象.findOne(查询条件[, 投影]) 找到了返回一个对象，没有找到返回null
>
> 例子：let rows = await student.find({age:{$gt:20}},{name:1,__id:0});

### 3.3 Update

> 模型对象.updateOne(查询条件, 要更新的内容[, 配置对象])
>
> 模型对象.updateMany(查询条件, 要更新的内容[, 配置对象])

### 3.4 Delete 

> 模型对象.deleteOne(查询条件)
>
> 模型对象.deleteMany(查询条件)
>
> - 注意如果不用await接收则不会执行

## 4. 模块化开发

- 把不同功能的代码 拆分成多个模块，方便维护和修改

### 4.1 连接数据库模块

- ```
  // 引入mongoose 模块
  let mongoose = require('mongoose');
  // collection.ensureIndex 即将弃用 设置useCreateIndex
  mongoose.set('useCreateIndex',true);
  // 数据库名字
  const DB_NAME = 'demo';
  //
  const DB_URL = 'localhost:27017'
  // 连接数据库 
  mongoose.connect(`mongodb://${DB_URL}/${DB_NAME}`, { useNewUrlParser: true });
  // 通过promise 解决 回调地狱的问题
  // 通过 module.exports 暴露外界所需代码
  module.exports = new Promise((resolve, reject) => {
      //监听连接状态
      mongoose.connection.on('open', (err) => {
          if (!err) {
              console.log(`来自${DB_URL}上的${DB_NAME}数据库连接成功了`);
              resolve();
          } else {
              reject(err);
          }
      })
  })
  ```

### 4.2 Mondel 模块

- ```
  let mongoose = require('mongoose');
  
  // 请来一个保安 ------------- 引入约束Schema
  let Schema = mongoose.Schema;
  
  // 制定一个进入你家的规则 -------------- 创建一个约束对象实例
  // 以students来举例
  let studentsSchema = new Schema({
      stu_id: {
          type: String, //类型
          required: true, //限制必填信息
          unique: true //限制该字段是唯一信息
      },
      name: {
          type: String,
          required: true,
      },
      sex: {
          type: String,
          required: true
      },
      hobby: [String], //该字段是以数组存储，并且数组中每个字段是以字符串的形式存储
      info: {
          type: Schema.Types.Mixed //接收所有类型
      },
      date: { // 注册时间
          type: Date,
          default: Date.now() // 默认 当前时间
      },
      enable_flag: { // 用来标记用户是否删除该数据，逻辑删除
          type: String,
          default: 'Y' // Y 是能展示 N是不能展示
      }
  });
  
  // 告诉保安你的规则 -------------创建模型对象
  // 第一个参数与数据库中的集合相对应，第二个参数指定约束对象实例
  module.exports = mongoose.model('students', studentsSchema);
  ```

### 4.3 业务模块 

- ```
  // 请来连接数据模块
  let db = require('./module/db');
  // 请来学生模块
  let studentMondel = require('./module/model/studentModel');
  
  ;(async () => {
      await db;
      // 业务逻辑
      let rows = await studentMondel.findOne({name:'小红'});
      console.log(rows);
  })();
  
  ```

# express 框架搭建服务器

## 1. 简单服务器搭建

- ```
  // 引入
  let express = require('express');
  // 不让express 在响应头中展示该框架名
  express().disable('x-powered-by');
  // 创建app服务
  let app = express();
  
  
  app.get('/',(request,response) => {// 根路由
      // 结束并返回结果
      response.send('你好');
  }).get('/meishi',(request,response)=> { // 一级路由
      response.send('你好，美食');
  }).listen(8080,(err)=>{
      if(!err) console.log('服务器启动成功了');
      else console.log(err)
  })
  ```

## 2. request 和 response 

### 2.1 request:

1. request.query  获取查询字符串的参数，拿到一个对象
2. request.params() 获取参数路由的参数，拿到一个对象
3. request.body 获取post请求体，拿到的是一个对象（要借助一个中间件）

### 2.2 response:

1. response.send() 给浏览器做出一个响应
2. response.end() 给浏览器做出一个响应（不会自动追加响应头，容易乱码）
3. response.download() 告诉浏览器下载一个文件（相对路径）
4. response.sendFile() 给浏览器发送一个文件 （绝对路径）如果发送的是浏览器能打开的则打开，不能则下载（例如 xxx.zip）
5. response.redirect() 重定向到一个新的网址（url）
6. response.set(header,value) 自定义响应头的内容
7. response.get() 获取响应头指定key对应的value
8. response.status(code) 设置响应状态码

## 3. Route（路由）

### 3.1 Route 是什么

- 路由是指如何定义应用的端点（URIs）以及如何响应客户端的请求。
- 路由是由一个URI、HTTP请求（GET、POST等）和若干个句柄组成

### 3.2 Route 的定义

- 我们可以将路由定义为三个部分
- 第一部分：HTTP请求的方法（get或者post）
- 第二部分：URI路径
- 第三部分：回调函数

## 4. 中间件

- 概念：本质上就是一个函数，包含三个参数：request，response，next

### 3.1 作用

1. 执行任何代码。
2. 修改请求和响应对象
3. 终结请求-响应循环
4. 调用堆栈中的下一个中间件

### 3.2 分类

1. 应用（全局）级中间件（过滤非法的请求，例如防盗链）

   - 第一种学法：app.use((request,response,next)=>{})
   - 第二种学法：使用函数定义

2. 第三方中间件(通过yarn 下载的中间件，例如body-parser)

   - app.use(bodyParser.urlencoded({extended:true}))

3. 内置中间件（express 内部封装好的中间件）

   - app.use(express.urlencoded({extended:true})) --- 当使用request.body 获取post请求体时要用的中间件

   - app.use(express.static('public')) ------暴露该文件夹下的静态资源

4. 路由器中间件（Router）

- 备注：
  - 在express中，定义路由和中间件的时候，根据定义的顺序（代码的顺序），将定义的每一个中间件或路由放在一个类似于数组的容器中，当请求过来的时候，一次中容器取出中间件和路由，进行匹配，如果匹配成功，交由该路由或者中间件处理

## 5. Router 路由器

### 5.1 Router 是什么？

- Router 是一个完整的中间件和路由系统，可以看做是一个小型的app 对象。

### 5.2 为什么使用Router

- 为了更好分类管理Route

### 5.3 Router 的使用

- 和模块化有点类似

- ```
  
  let {Router} = require('express');
  
  let router = new Router()
  
  router.get('/login',(req,res)=>{
  	//body....
  })
  
  // 暴露出去
  module.exports = router
  ```

- 要在主server文件中引入

  - ```
    // 引入
    let uiRouter = require('./router/uiRouter');
    // 使用
    app.use(uiRouter);
    // 注意路由中的文件路径，有path模块拼接路由
    ```

# cookie

## 1. 是什么？

- 本质就是一个字符串，里面包含着浏览器和服务器沟通的信息（交互时产生的信息）。
- 储存形式以：key-value的形式
- 浏览器和自动携带该网站的cookie，只要是该网站下的cookie，全部携带。

## 2. 分类

- 会话cookie （关闭浏览器后，会话cookie会自动消失，会话cookie存储在浏览器运行的那块内存上）。
- 持久化cookie：（看过期时间，一旦到了过期时间，自动销毁，存储在用户的硬盘上，但是用户清理缓存等操作就会消失）。

## 3. 工作原理：（就好比是小纸条）

- 当浏览器第一次请求服务器的时候，服务器可能返回一个或多个cookie给浏览器，浏览器判断以下cookie种类
  - 会话cookie ：存储在浏览器运行的那块内存上
  - 持久化cookie： 存储在硬盘上
- 以后请求该网站的时候，自动携带上该网站的所有cookie（无法进行干预）
- 服务器拿到之前自己"种"下cookie，分析里面的内容，校验cookie的合法性，根据cookie里保存的内容，进行具体的业务逻辑。

## 4. 应用：

- 解决http无状态问题（例子：7天免登陆，一般来说不会单独使用cookie，一般配合后台的session存储使用）
- 不同的语言，不同的后端框架cookie的具体语法是不一样的，但是cookie原理和工作过程是不变的。
- 备注：cookie不一定只由服务器生成，前端同样可以生成cookie，但是前端生成的cookie几乎没有意义

## 5. 使用：

- “种”下会话cookie ： response.cookie(key,value)
- “种”下持久化cookie ： response.cookie(key,value,}{maxAge:时间})

- 获取cookie，要载入 cookie-parser 中间件

  - yarn add cookie-parser

  - 第一步：引入   let cookieParser = require('cookie-parser');
  - 第二步：app.use(cookieParser())
  - 第三步：const {cookie名称} = request.cookies;

- 删除cookie

  - 第一种：response.cookie('cookie名称','',{maxAge:0})
  - 第二种：response.clearCookie('cookie名称'')

# session

## 1. 是什么

- 标准来说，session指的是会话，但是后端人员常说的session，全称叫：服务器session存储。

## 2. 特点

- 存在于服务端
- 存储的是浏览器和服务器之间沟通产生的一些信息
- 默认session的存储在服务器的内存中，每当一个新客户端发来请求，服务器都会新开辟出一块空间，供session会话存储使用。

## 3. 工作流程

- 第一浏览器请求服务器的时候，服务器会开辟出一块内存空间，供session会话存储使用。返回响应的时候，会自动返回一个cookie（有时候回返回多个，为了安全），cookie里包含着，上一步产生会话存储“容器”的编号（id），以后请求的时候，会自动携带这个cookie，给服务器。服务器从该cookie中拿到对应的session的id，去服务器中匹配。服务器会根据匹配信息，决定下一步具体的业务逻辑。
- 备注：一般来说cookie一定会配合session使用

## 4. 使用

1. 下载安装：

   - yarn add express-session // 用于在express中操作session

2. 下载安装：

   - yarn add connect-mongo // 用于将session写入数据库（session持久化）

3. 引入express-session模块： 

   - const session = require('express-session')

4. 引入connect-mongo模块：

   - const mongoStore = require('connect-mongo')(session);

5. 编写全局配置对象：

   - ```
     app.use(session({
     	name: 'userid', //设置cookie的name，默认值是：cnnect.sid
     	secret: 'atguigu', //参与加密的字符串（又称签名）
     	saveUninitialized: false, //是否在存储内容之前创建会话
     	resave: true, //是否在每次请求时，强制重新保存session，即使他们没变化
     	store: new mongoStore({
     		url: 'mongodb://localhost:27017/cookies_container',
     		touchAfter: 24 * 3600 //修改频率（例：//在24小时之内只更新一次）
     	}),
     	cookie:{
     		httpOnly: true, // 开启后前端无法通过js操作cookie
     		maxAge: 1000 * 30 //设置cookie的过期时间
     	}
     }));
     6.向session中添加一个xxxx，值为yyy: req.session.xxxx = yyy
     7. 获取session上的xxx属性：let {xxx} = req.session
     ```

   - 