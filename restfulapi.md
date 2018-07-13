# REST 学习笔记

### 名字起源
REST -- REpresentational State Transfer首先，之所以晦涩是因为前面主语被去掉了，全称是 Resource Representational State Transfer：通俗来讲就是：资源在网络中以某种表现形式进行状态转移。

### 解决什么问题
主要解决url命名混乱，url中包含操作这种混乱的接口设计方法。从而达到：
* 看Url就知道要什么
* 看http method就知道干什么
* 看http status code就知道结果如何

### 六个限制
1. 客户-服务器（Client-Server）客户端服务器分离 
    优点，提高用户界面的便携性（操作简单） 通过简化服务器提高可伸缩性（高性能，低成本） 允许组件分别优化（可以让服务端和客户端分别进行改进和优化） 
2. 无状态（Stateless） 从客户端的每个请求要包含服务器所需要的所有信息 
    优点： 提高可见性（可以单独考虑每个请求） 提高了可靠性（更容易从局部故障中修复） 提高可扩展性（降低了服务器资源使用） 
3. 缓存（Cachable） 服务器返回信息必须被标记是否可以缓存，如果缓存，客户端可能会重用之前的信息发送请求。 
    优点： 减少交互次数 减少交互的平均延迟 
4. 分层系统（Layered System） 系统组件不需要知道与他交流组件之外的事情。封装服务，引入中间层。 
    优点： 限制了系统的复杂性 提高可扩展性 
5. 统一接口（Uniform Interface） 
    优点： 提高交互的可见性 鼓励单独改善组件 
6. 支持按需代码（Code-On-Demand 可选） 
    优点： 提高可扩展性 

### 注意事项
1. API versioning:可以放在URL里面，也可以用HTTP的header：
2. URI使用名词而不是动词，且推荐用复数。
3. 保证 HEAD 和 GET 方法是安全的，不会对资源状态有所改变（污染）。
4. 资源的地址推荐用嵌套结构。
5. 警惕返回结果的大小。如果过大，及时进行分页（pagination）或者加入限制（limit）。HTTP协议支持分页（Pagination）操作，在Header中使用 Link 即可。
6. 使用正确的HTTP Status Code表示访问状态。

### 最重要的要求
![](https://pic1.zhimg.com/80/7405939b62a73f28846533de08db3a80_hd.jpg)<br>
用HTTP协议里的动词来实现资源的添加，修改，删除等操作。即通过HTTP动词来实现资源的状态扭转：
* GET    用来获取资源，
* POST  用来新建资源（也可以用于更新资源），
* PUT    用来更新资源，
* DELETE  用来删除资源。
[Using HTTP Methods for RESTful Services](https://www.restapitutorial.com/lessons/httpmethods.html)

