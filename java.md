### idea中配置tomcat
点击Run->Edit Configuration，左上角+，选择tomcat server local进行配置，然后切换标签Deployment，点击+，选择artifacts，如果没有，则点击File->Project Structure，Artifacts选项，点击+，选择web application：exploded，Modules选项，Dependencies标签，点击+，可添加外部依赖库。

### Java web项目使用mysql数据库
需要将mysql-connector-java.jar拷贝到tomcat下的lib目录里

### spring中的一些术语
DI:依赖注入（dependency injection）
AOP:面向切面编程（aspect-oriented programming）
EJB:企业级JavaBean
JDO:Java数据对象
POJO：简单老式Java对象（Plain Old Java object）

### spring的核心概念
1. 利用DI实现组件之间的松耦合，利用AOP实现横切关注点业务的可重用。
2. 装配bean时优先使用自动配置、其次使用JavaConfig、最后选择XML方式。
