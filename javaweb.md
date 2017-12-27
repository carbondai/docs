### 发布javaweb项目的流程
将开发测试完成后的项目打包成war格式，在idea中流程为build->artifacts->war:archive，在output目录下会找到.war文件，在服务器端部署tomcat，将war包拷贝至tomcat下的webapps目录中，运行tomcat即可

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

### springMVC和Mybatis开发Javaweb项目
接口加配置文件模式编程。
controller类中：
1. 用@Autowired自动装载service接口
2. 用@RequestMapping实现get方法，在方法体中用model.addAttribute()将service类中的操作方法返回对象与jsp页面中要展示的变量名关联起来，get方法的返回值为jsp地址的中间部分
3. 用@RequestMapping实现post方法，在其参数中加上method=RequestMethod.POST，同时加上produces={"application/json;charset=UTF-8"}指定文件格式和编码，然后添加@ResponseBody注解，在方法体中用service类的操作方法操作对象
4. 每个service接口有一个对应的实现类，在service接口的实现类中，装载了Mapper接口，其中定义的方法名与Mybatis对应定义的Mapper.xml文件中的sql语句的id一致
5. 通过在spring.xml中配置sqlSessionFactory和MapperScannerConfigurer，将Mapper接口和Mapper.xml文件关联起来
下面以代码为例，展示用法：
```
GoodsController.java
/**
 *商品展示
 */
@Controller
public class GoodsController
{
	@Autowired
	private GoodsService goodsService;
	
	@RequestMapping("/buy")
	public String buy(Model model, String detailgId)
	{
		goodsList.clear();
		goodsList.add(goodsService.findDetail(detailgId));
        
		model.addAttribute("buyingList", goodsList);
		return "/goods/buy";
	}
}
```

```
GoodsService.java
public interface GoodsService
{
    /** find all goods **/
    List<Goods> findAllGoods();

    /** find goods by keyword*/
    List<Goods> queryByKeyword(String keyword);

    /** find goods by id*/
    Goods findDetail(String gId);
}

GoodsServiceImpl.java
@Service
public class GoodsServiceImpl implements GoodsService
{
    @Autowired
    private GoodsMapper goodsMapper;

    @Override
    public List<Goods> findAllGoods()
	{
        return goodsMapper.selectAll();
    }

    @Override
    public List<Goods> queryByKeyword(String keyword)
    {
        return goodsMapper.selectByCondition(keyword);
    }
    
    @Override
    public Goods findDetail(String gId)
    {
        return goodsMapper.selectBygId(Integer.parseInt(gId));
    }    
}
```

```
GoodsMapper.java
public interface GoodsMapper {
    
    List<Goods> selectAll();

    List<Goods> selectByCondition(@Param("keyword") String keyword);

    Goods selectBygId(int gId);
}
```

```
GoodsMapper.xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="lyons.common.mapper.goods.GoodsMapper">
  <resultMap id="BaseResultMap" type="Goods">
    <result column="G_ID" jdbcType="INTEGER" property="gId" />
    <result column="G_NAME" jdbcType="VARCHAR" property="gName" />
    <result column="G_DESCRIBE" jdbcType="VARCHAR" property="gDescribe" />
    <result column="G_PRICE" jdbcType="DOUBLE" property="gPrice" />
    <result column="G_MADE" jdbcType="VARCHAR" property="gMade" />
    <result column="G_AMOUNT" jdbcType="INTEGER" property="gAmount" />
    <result column="G_CREATE_DATE" jdbcType="TIMESTAMP" property="gCreateDate" />
    <result column="G_PIC" jdbcType="VARCHAR" property="gPic" />
  </resultMap>
  
  <sql id="Base_Column_List">
    G_ID, G_NAME, G_DESCRIBE, G_PRICE, G_MADE, G_AMOUNT, G_CREATE_DATE, G_PIC
  </sql>
  
  <select id="selectAll" resultMap="BaseResultMap">
    SELECT
    <include refid="Base_Column_List" />
    FROM goods
  </select>
  
  <select id="selectBygId" parameterType="INTEGER" resultMap="BaseResultMap">
    SELECT
    <include refid="Base_Column_List" />
    FROM goods
    where g_id = #{gId}
  </select>
  
  <select id="selectByCondition" parameterType="String" resultMap="BaseResultMap">
  	SELECT
    <include refid="Base_Column_List" />
    FROM goods
    WHERE g_name LIKE '%'|| #{keyword} ||'%'
  </select>
```

```
spring.xml
<!--3.配置SqlSessionFactory对象-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--往下才是mybatis和spring真正整合的配置-->
        <!--注入数据库连接池-->
        <property name="dataSource" ref="dataSource"/>
        <!--配置mybatis全局配置文件:mybatisConfig.xml-->
        <property name="configLocation" value="classpath:config/mybatis/mybatisConfig.xml"/>
        <!--扫描entity包,使用别名,多个用;隔开-->
        <property name="typeAliasesPackage" value="lyons.common.model"/>
        <!--扫描sql配置文件:mapper需要的xml文件-->
        <property name="mapperLocations" value="classpath:config/mybatis/*Mapper.xml"/>
    </bean>
    
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory" />
	</bean>
	
    <!-- 4:配置扫描Dao接口包,动态实现DAO接口,注入到spring容器-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入SqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!-- 给出需要扫描的Dao接口-->
        <property name="basePackage" value="lyons.common.mapper.*"/>
    </bean>
```
    
        
    

