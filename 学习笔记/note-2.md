以前开发每太深入，最近自己独立做项目，遇到很多模糊的概念，这边做一个记录

### route和router 到底啥关系
#### 1.什么route
route 简单来说是路由，就是路径到函数的映射，它是将一个 url 路径和一个函数进行映射，例如：

  /users   -->getAllusers()
  /users/count ---->getUsersCount()
  
#### 2.什么是router
router 是一个路由器,可以理解为一个容器, 或者是一种机制,管理着一组route

总的来说，route 只是进行 url 到函数的映射, 但是当接收到一个 url 之后, 去路由映射表查找响应的函数，这个过程是由router来处理的
