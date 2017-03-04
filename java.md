### idea中配置tomcat
点击Run->Edit Configuration，左上角+，选择tomcat server local进行配置，然后切换标签Deployment，点击+，选择artifacts，如果没有，则点击File->Project Structure，Artifacts选项，点击+，选择web application：exploded，Modules选项，Dependencies标签，点击+，可添加外部依赖库。

### Java web项目使用mysql数据库
需要将mysql-connector-java.jar拷贝到tomcat下的lib目录里

### spring中的一些术语
DI:依赖注入（dependency injection），又叫做控制反转（IoC）
AOP:面向切面编程（aspect-oriented programming）
EJB:企业级JavaBean
JDO:Java数据对象
POJO：简单老式Java对象（Plain Old Java object）

### spring的核心概念
1. 利用DI实现组件之间的松耦合，利用AOP实现横切关注点业务的可重用。
2. 装配bean时优先使用自动配置、其次使用JavaConfig、最后选择XML方式。

### servlet概念
Java web开发中，需要编写Java类实现servlet接口，然后将开发好的Java类部署到Tomcat之类的web服务器中。通常将实现了servlet接口的java程序，成为Servlet。
1. 浏览器发出http请求后，
2. 与之相连接的web服务器通过http报文解析出客户端想访问的主机、web应用和web资源，
3. 如果发现servlet是第一次访问，服务器就加载该servet并创建servlet对象，调用其init方法，
4. 创建一个用于封装http请求消息的HttpServletRequest对象和一个代表http响应消息的HttpServletResponse对象，调用Servlet的service（）方法并将请求和响应对象作为参数传递进去，
5. web服务器取出response数据回送给浏览器。
