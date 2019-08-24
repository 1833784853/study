# MongoDB与Mongoose

## 1. 数据库分类

### 1.1 关系型数据库（RDBS）

- **代表有**： MySQL、Oracle、DB2、SQL Server....
- **特点**：关系紧密，都是表
- **优点**：
  - **易于维护**：都是使用表结构，格式一致
  - **使用方便**： SQL语言通用，可用于复杂查询
  - **高级查询**：可用于一个表以及多个表之间非常复杂的查询
- **缺点**
  - 读写性能比较差，尤其是海量数据的高效读写
  - 有固定的表结构，字段不可随意更改，灵活度稍欠
  - 高比发读写需求，传统关系型数据库来说，硬盘I/O是一个很大的瓶颈

### 1.2 非关系型数据库（NoSQL）

- **代表有**：MongoDB、Redis...
- **特点**: 关系不紧密，有文档，有键值对
- **优点：**
  - **格式灵活**： 储存数据的格式可以是key，value形式。
  - **速度快**：nosql 可以内存作为载体，而关系型数据库只能使用硬盘
  - **易用**：nosql 数据库部署简单
- **缺点：**
  - 不支持sql，学习和使用成本较高；
  - 不支持事务 （事务：原子性（不可分割性））；
  - 复杂查询时语句过于繁琐

## 2. 安装

- 下载[Mongodb](https://www.mongodb.com/download-center/community)
- 配置变量环境
- 创建数据库位置
  - 可以是任何地方
  - \data\db
- window在4版本之后，只要找到mongo.exe,用管理员身份运行自动创建服务

---

## 3. 可视化工具

- **studio-3T**
  - 有15天免费试用
  - 破解方法：[地址](https://www.jianshu.com/p/7257f15e2620?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes)

## 4.MongoDB 原生CRUD（增删改查）命令总结

### creat :

> db.集合名.insert(文档对象)
>
> db.集合名.insertOne(文档对象)
>
> db.集合名.insertMany([文档对象, 文档对象])

- 如果集合名不存在就会自动创建，并添加文档

### read：

> **db.集合名.find(查询条件[, 投影])**
>
> - **投影：过滤掉不想要的数据，只保留想要展示的数据**
>   - 举例：db.students.find({},{_id:0,name:0}) //过滤掉id和name，**注意了如果一对键值对出现了0就不能有1, _id除外**
>   - 举例：db.students.find({},{age:1}) //只保留age
>
> - 举例：db.students.find({age:18}) //查询年龄为18的所有信息
> - 举例：db.students.find({age:18,name:'小明'}) //查询年龄为18且名字为小明的学生
>
> **补充 ： db.集合体.findOne(查询条件[, 投影])**
>
> **常用操作符**：
>
> - **< , <= , > , >= , !== 对应为：$lt $lte $gt $gte $ne**
>
>   - 举例：db.students.find({age:{$gte:20}}) //查询年龄大于等于20的学生
>
> - **逻辑或：使用$in 或 $or**
>
>   - 查询年龄为18或20的学生
>   - 举例：db.students.find({age:{$in:[18,20]}})
>   - 举例：db.students.find({$or:{age:18},{age:20}})
>
> - **逻辑非：&nin**
>
> - **正则匹配**
>
>   - 举例: db.students.find({name:/^T/}) //查询名字以T开头的全部信息
>
> - **$where能写函数**
>
>   - db.students.find({$where: function() {
>
>     ​		return this.name === '小明' && this.age === 18
>
>     }})

### update：

> **db.集合体.update(查询条件,要更新的内容[, 配置对象])**
>
> - **// 如下会将更新内容替换掉整个文档对象，但_id 不受影响**
>   - 举例：db.students.update({name:'张三'},{age:18})
> - **//使用$set 修改指定内容，其他数据不变，不过只能匹配一个张三**
>   - 举例：db.students.update({name:'张三'},{$set:{age:20}})
> - **//修改多个文档对象，匹配多个张三，把所有的张三的年龄都替换成30**
>   - 举例：db.students.update({name:'张三'},{$set:{age:30}},{multi:true})
> - **补充：**
>   - db.集合名。updateOne(查询条件，要更新的内容[, p配置对象])
>   - db.集合名。updateMany(查询条件，要更新的内容[, p配置对象])

### delete：

> **db.集合名.remove(查询条件)**
>
> - // 删除所有年龄小于等于19的学生
>   - 举例：db.students.remove({age:{$lte:19}})

## 5. Mongoose 的使用

### 5.1 简介

- Mongoose 是一个对象文档模型（ODM）库，它对Node 原生的 MongoDB 模块进行了进一步的优化封装，并提供了更好的功能。

### 5.2 优势

- 可以为文档创建一个模式结构（Schema）
- 可以对模型中的对象、文档进行验证
- 数据可以通过类型转换转换为对象模型
- 可以使用中间件来应用业务逻辑挂钩
- 比Node 原生的 MongoDB 驱动更容易

### 5.3 使用

```node

```

