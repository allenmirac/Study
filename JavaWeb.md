## 大苏打

### MyBatis 的配置文件

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

- configuration（配置）
  - [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
  - [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
  - [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
  - [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
  - [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
  - [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
  - [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

### SSM框架中Dao层，Mapper层，controller层，service层，model层，entity层都有什么作用

SSM是sping + springMVC+mybatis集成的框架。

MVC即model view controller。

model层=entity层。存放我们的实体类，与数据库中的属性值基本保持一致。

service层。存放业务逻辑处理，也是一些关于数据库处理的操作，但不是直接和数据库打交道，他有接口还有接口的实现方法，在接口的实现方法中需要导入mapper层，mapper层是直接跟数据库打交道的，他也是个接口，只有方法名字，具体实现在mapper.xml文件里，service是供我们使用的方法。

mapper层=dao层，现在用mybatis逆向工程生成的mapper层，其实就是dao层。对数据库进行数据持久化操作，他的方法语句是直接针对数据库操作的，而service层是针对我们controller，也就是针对我们使用者。service的impl是把mapper和service进行整合的文件。

（多说一句，数据**持久化操作**就是指，把数据放到持久化的介质中，同时提供增删改查操作，比如数据通过hibernate插入到数据库中。）

controller层。控制器，导入service层，因为service中的方法是我们使用到的，controller通过接收前端传过来的参数进行业务操作，在返回一个指定的路径或者数据表。

### java web目录结构：

src下面：

com.etc.controller：写servelet.java，如：LoginServelet.java

com.etc.dao：写实体的接口，AdminDao.java

com.etc.dao.impl:  写上面Dao接口实体的实现，AdminDaoImpl.java

com.etc.entity:  写数据库的实体，Admin.java

com.etc.service:  写服务，是接口，实际上就是与controller连接的接口，

com.etc.service.impl:  写上面service接口的实现，重写service interface，实际上就是return DaoImpl里面的方法，如：return us.queryUsersAll();

com.etc.test:  写dao.impl的方法的测试过程

com.etc.util:  数据库统一的接口：下面有一个文件，DBUtil.java



### Mybatis数据操作

两步走：

1、编写Mapper方法，即dao层，如：List<Brand> selectAll(); 

2、然后在Mybatis的映射文件里面编写映射的SQL语句：

<select id="selectAll" resultType="Brand">

select * from tb_brand

</select>

然后测试一下是否实现了。

**在Mybatis里面编写方法的映射文件相当于编写了 impl，实现了方法**

### **session.setAttribute**

setAttribute这个方法，在JSP对象中的session和request都有这个方法，这个方法作用就是保存数据，然后还可以用getAttribute方法来获取出来。request作用域中，然后在**转发进入的页面**就可以获取到你的值，如：request.setAttribute("users",users)这个方法是将users这个对象保存在request作用域中，然后在跳转的界面就可以使用users对象。

### java Web 工程servlet中@WebServlet("/HelloServlet") 是怎么工作的

编写好Servlet之后，接下来要告诉Web容器有关于这个Servlet的一些信息。在Servlet 3.0中，可以使用标注(Annotation)来告知容器哪些Servlet会提供服务以及额外信息。例如在HelloServlet.java中：
@WebServlet("/hello.view")
public class HelloServlet extends HttpServlet {
只要在Servlet上设置@WebServlet标注，容器就会自动读取当中的信息。上面的@WebServlet告诉容器，如果请求的URL是“/hello.view”，则由HelloServlet的实例提供服务。可以使用@WebServlet提供更多信息。

**补充添加：标注(Annotation)声明后，则不需要在Web.xml中再次声明servlet的相关信息了（web.xml）在WEB-INF文件夹下**







