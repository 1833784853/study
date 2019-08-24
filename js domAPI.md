str.indexOf("参数")：找到字符串中和参数一样的字符串，返回的是出现的索引，找不到返回-1。

str.startsWith("参数")：判断字符串是否以参数开头，返回的是布尔类型。
str.endsWith("参数")：判断字符串是否以参数结尾，返回的是布尔类型。

H5中新增操作DOM方法

选择器：
document.querySelector('参数1') ：如果获取的是class则带. id则带# 标签什么都不带
document.querySelectorAll('参数1')：获取是一个数组

操作class属性：

.classList ：获取的是元素的全部样式（数组）
.add("class名称") ：如果要添加两个样式要写两遍，列如document.querySelector('li').classList.add('rad')
document.querySelector('li').classList.add('blue')
.remove('class名称')：移除class样式，不是移除class属性，例如document.querySelector('li').classList.remove('rad')
.toggle('class名称'):如果class样式没有则添加，有则删除，例如document.querySelector('li').classList.toggle('rad')
.contains('class名称'): 判断元素是否包含指定名称的样式，返回true或false 例如document.querySelector('li').classList.contains('rad')

local储存
localStorage.setItem()
