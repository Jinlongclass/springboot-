# SpringBoot学习笔记

## 1.该如何学习SpringBoot

![1596692351087](C:\Users\jiangjinlong\AppData\Roaming\Typora\typora-user-images\1596692351087.png)

**SpringBoot：6天**

**SpringCloud：3天**

## 2.什么是SpringBoot

​	就是一个javaweb的开发框架，约定大于配置。约定优于配置（Convention Over Configuration）,也称作按约定编程是一种软件设计范式。目的在于减少软件开发人员所需要做出的决定的数量，从而获得简单的好处，而又不失去其中的灵活性。开发人员仅仅需要规定应用中不符合约定的部分。例如，如果模型中有个名为Sale的类，数据库中对应的表就会默认命名为sales。只有在偏离这一约定的时候,比如将该表命名为"products_sold"，才会需要写有关这个名字的配置。如果所用工具的约定与你的期待相符，便可省去配置；反之，你可以配置来达到你所期待的方式。

## 3.什么是微服务架构

## 4.第一个SpringBoot程序

​	**可以选择在网站上面生成SpringBoot项目，也可以在IDEA中，之间创建SpringBoot项目。**

## 5.SpringBoot原理部分

​	**自动装配**：pom.xml

​		springboot核心依赖在父程中

​		我们在写或者引入一些springboot依赖的时候，不需要指定版本，就因为有这些版本仓库

​	**启动器**:starter

​		实质就是springboot的启动场景。

​		比如spring-boot-starter-web，直接自动导入web环境的所有依赖

​		我们要使用什么场景只要找到对应的启动器

​		对应的springboot启动器查找地址：

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```

​	**主程序**

​		@SpringBootApplication注解：标注这个类是一个springboot程序，必须存在

```java
@SpringBootApplication
public class Springboot01Application {

    public static void main(String[] args) {

        SpringApplication.run(Springboot01Application.class, args);
    }

}
```

**注解**

```java
@SpringBootConfiguration:	Springboot的配置，使得启动类下面的所有资源被导入
    @Configuration:	说明他是一个Spring配置类
    @Component：	说明它也是一个spring的组件而已

@EnableAutoConfiguration：	自动配置
    @AutoConfigurationPackage
	@Import(AutoConfigurationImportSelector.class)
```

**结论：**

​	Springboot所有的自动配置都是在启动的时候扫描并加载：Spring.factories所有自动配置类都在这里，但是不是全部都会生效，要判断条件是否成立，只要导入对应的starter，就有对应的启动器，我们自动装配就会生效，然后配置完成。

​	1.springboot在启动时，从类路径下/META-INF/spring.factories获取指定的值

​	2.将有些自动配置的类导入容器，自动配置会自动生效，帮助我们进行配置

​	3.以前我们自动配置的东西，现在springboot帮助我们完成

​	4.整合javaEE，解决方案和自动配置的东西都在

## 	**6.springBoot配置文件中到底能写什么**

**配置文件和spring.factories有什么联系**

@Configuration //表示这是一个配置类

@EnableConfigurationProperties //自动配置注解

@ConditionalOnWebApplication  //Spring的底层注解，根据不同的条件，来判断配置的类是否生效

在我们这个配置文件中可以配置的，都存在一个故有规律，都存在一个xxxProperties：和配置文件绑定   xxxAutoConfiguration:默认值，从而完成使用自定义的配置，按照固定的规则进行修改内容

**重点：自动装配的原理**

1、SpringBoot启动会加载大量的自动配置类：spring.factories中

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性； 利用springboot配置文件进行修改属性，从而实现自动注入** 

- **可以使用debug=true来判断哪些自动配置类生效**，** 【演示：查看输出的日志】 **

  **Positive matches:（自动配置类启用的：正匹配）**

  **Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）**

  **Unconditional classes: （没有条件的类）**

## **7.Springboot的web开发**

- **jar：webapp**
- **自动装配**
- **springboot到底能配置什么？我们需要修改什么？能修改什么？能不能扩展？**

要解决的问题：

- 导入静态资源

- 首页

- jsp：模板引擎 Thymeleaf

- 装配扩展SpringMVC

- 增删改查

- 拦截器

- 国际化

### **7.1静态资源**

**静态资源：**一般客户端发送请求到web服务器，web服务器从内存在取到相应的文件，返回给客户端，客户端解析并渲染显示出来。

**动态资源：**一般客户端请求的动态资源，先将请求交于web容器，web容器连接数据库，数据库处理数据之后，将内容交给web服务器，web服务器返回给客户端解析渲染处理。

**静态资源和动态资源的区别**

- a.静态资源一般都是设计好的html页面，而动态资源依靠设计好的程序来实现按照需求的动态响应；
- b.静态资源的交互性差，动态资源可以根据需求自由实现；
- c.在服务器的运行状态不同，静态资源不需要与数据库参于程序处理，动态可能需要多个数据库的参与运算。

**什么是webjars：**获取静态资源的方式： http://localhost:8080/webjars/jquery/3.5.1/jquery.js ,

**注意：导入静态资源，一般放置在static中**

- 在springboot中使用webjars来导入静态资源 访问方式： http://localhost:8080/webjars/jquery/3.5.1/jquery.js（很少使用）
- public、static、/**、resources     访问方式：localhost8080/内容
- 优先级：resources>static>public

### **7.2首页定制**

如何访问首页：首页的index.html只能放置在resources的下面的静态资源包内，放置在public下面；

首页：localhost：8080

在templates目录下面的所有页面只能通过controller跳转，需要模板引擎

### **7.3模板引擎**

Thymeleaf：用于使用controller跳转到界面，**加载静态资源**

**注意：页面放置在templates文件目录下**

 		模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。其他的我就不介绍了，我主要来介绍一下SpringBoot给我们推荐的Thymeleaf模板引擎，这模板引擎呢，是一个高级语言的模板引擎，他的这个语法更简单。而且呢，功能更强大。 

**只要使用thymeleaf，只需要导入对应的依赖即可，注意版本，完成之后，将html文件放在templates文件下即可**

```xml
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
</dependency>
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-java8time</artifactId>
</dependency>
```

**thymeleaf语法：**

- Simple expressions:
  - Variable Expressions: `${...}`
  - Selection Variable Expressions: `*{...}`
  - Message Expressions: `#{...}`
  - Link URL Expressions: `@{...}`
  - Fragment Expressions: `~{...}`
- Literals
  - Text literals: `'one text'`, `'Another one!'`,…
  - Number literals: `0`, `34`, `3.0`, `12.3`,…
  - Boolean literals: `true`, `false`
  - Null literal: `null`
  - Literal tokens: `one`, `sometext`, `main`,…
- Text operations:
  - String concatenation: `+`
  - Literal substitutions: `|The name is ${name}|`
- Arithmetic operations:
  - Binary operators: `+`, `-`, `*`, `/`, `%`
  - Minus sign (unary operator): `-`
- Boolean operations:
  - Binary operators: `and`, `or`
  - Boolean negation (unary operator): `!`, `not`
- Comparisons and equality:
  - Comparators: `>`, `<`, `>=`, `<=` (`gt`, `lt`, `ge`, `le`)
  - Equality operators: `==`, `!=` (`eq`, `ne`)
- Conditional operators:
  - If-then: `(if) ? (then)`
  - If-then-else: `(if) ? (then) : (else)`
  - Default: `(value) ?: (defaultvalue)`

**使用方式：所有的html元素都要被thymeleaf接管**，**接管方式为** th：元素名=“${}”，元素名有text、class

```html
<span th:text="${today}">
<div th:text="${msg}"></div>
```

**遍历实现方式**

​	1.在controller类中写model，

```java
 model.addAttribute("user", Arrays.asList("jinlong","naiyou"));
```

​	2.在html文件中写如何进行遍历读取

```html
<hr>
<h3 th:each="user:${user}" th:text="${user}"></h3>
```

### **7.4装配扩展SpringMVC**

加入我们需要扩展springmvc。官方建议我们这样做：

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    //视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/jiang").setViewName("test");
    }
}
```

@EnableWebMvc实质就是导入一个类，当我们要扩展SpringMvc时，千万不能添加这个注解，读源码去看 WebMvcAutoConfiguration，一点点来

**备注**：在springboot中，存在很多的xxxxConfiguration配置类，需要看到底扩展了哪些功能

### **7.5员工信息管理系统的开发**

​	**1.导入相关资源，在static中导入静态资源**

​	![1597220249144](C:\Users\jiangjinlong\AppData\Roaming\Typora\typora-user-images\1597220249144.png)

**2.在templates目录下导入相关html页面**

​	![1597220293142](C:\Users\jiangjinlong\AppData\Roaming\Typora\typora-user-images\1597220293142.png)

**3.新建pojo包，写实体类：Department、Employee**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Department {
    private Integer id;
    private String deparmentName;
}
```

```java
@Data
@NoArgsConstructor

public class Employee {
    private Integer id;
    private String lastName;
    private String email;
    private Integer gender;
    private Department department;
    private Date birth;

    public Employee(Integer id, String lastName, String email, Integer gender, Department department, Date birth) {
        this.id = id;
        this.lastName = lastName;
        this.email = email;
        this.gender = gender;
        this.department = department;
        //默认创建日期
        this.birth = new Date();

    }
```

**4.建dao包，见实体类对应的dao类：DepartmentDao、EmployeeDao**

```java
@Repository
public class DepartmentDao {
    //模拟数据库中的数据
    private static Map<Integer, Department> departments=null;
    static{
        departments=new HashMap<Integer, Department>();
        departments.put(101,new Department(101,"教学部"));
        departments.put(102,new Department(102,"学生部"));
        departments.put(103,new Department(103,"运营部"));
        departments.put(104,new Department(104,"市场部"));
        departments.put(105,new Department(105,"后勤部"));
    }
    //获得所有部门信息
    public Collection<Department> getinfo(){
        return departments.values();
    }
    //获取得到部门
    public Department getDepartmentById(Integer id){
        return departments.get(id);
    }

}
```

```java
public class EmployeeDao {
    //模拟员工数据操作
    private static Map<Integer, Employee> employee=null;
    private DepartmentDao departmentDao;
    static{
        employee=new HashMap<Integer, Employee>();
        employee.put(1001,new Employee(1001,"AA","AA123456",1,new Department(101,"教学部")));
        employee.put(1002,new Employee(1002,"BB","BB123456",0,new Department(102,"学生部")));
        employee.put(1003,new Employee(1003,"CC","CC123456",1,new Department(103,"运营部")));
        employee.put(1004,new Employee(1004,"DD","DD123456",0,new Department(104,"市场部")));
        employee.put(1005,new Employee(1005,"EE","EE123456",1,new Department(105,"后勤部")));
    }
    //获取员工信息
    public Collection<Employee> getinfo(){
        return employee.values();
    }
    //根据员工ID获得员工信息
    public Employee getinfoById(Integer id){
        return employee.get(id);
    }
    //增加一个员工信息
    //主键自增
    private static Integer tempid=1006;
    public void InsertEmployee(Employee employees){
        if(employees.getId()==null){
            employees.setId(tempid++);
        }
        employees.setDepartment(departmentDao.getDepartmentById(employees.getDepartment().getId()));
        employee.put(employees.getId(),employees);
    }
    //删除员工信息
    public void Deleteinfo(Integer id){
        employee.remove(id);
    }


}
```

**5.首页映射**

​		关于首页映射展示，可以通过创建controller类来进行控制首页显示，还可以通过重写视图跳转函数，添加视图首页跳转

​		首页配置：所有的页面的静态资源必须要由thymeleaf来接管，否则无法显示出来静态资源：@{}

```java
@Controller
public class IndexController {
    @RequestMapping({"/","/index.html"})
    public String index(){
        return "index";
    }

}
```

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/index.html").setViewName("index");
    }
}
```

**修改所有的html文件，使得其满足thymeleaf控制要求，从而能够让静态资源能够加载**

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
		<meta name="description" content="">
		<meta name="author" content="">
		<title>Signin Template for Bootstrap</title>
		<!-- Bootstrap core CSS -->
		<link th:href="@{/css/bootstrap.min.css}" rel="stylesheet">
		<!-- Custom styles for this template -->
		<link th:href="@{/css/signin.css}" rel="stylesheet">
	</head>

	<body class="text-center">
		<form class="form-signin" th:action="@{/user/login}" method="post">
			<img class="mb-4" th:src="@{/img/bootstrap-solid.svg}" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
			<!--判断-->
			<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
			<label class="sr-only"th:text="#{login.username}">Username</label>
			<input type="text" name="username" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
			<label class="sr-only" th:text="#{login.password}">Password</label>
			<input type="password" name="password" class="form-control" th:placeholder="#{login.password}" required="">
			<div class="checkbox mb-3">
				<label>
          <input type="checkbox" value=""> [[#{login.remember}]]
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit">[[#{login.btn}]]</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
			<a class="btn btn-sm"th:href="@{/index.html(l='en_US')}">English</a>
		</form>

	</body>

</html>
```



```java
#关闭themeleaf引擎的缓存
spring.thymeleaf.cache=false
```

**6.页面国际化**

即中英文切换

注意点：

​	1.配置i18n文件



​	2.如果需要按钮自动切换，需要自定义一个组件LocalResolver。实现LocalResolver接口，重写函数。记住要将自己写的组件配置到spring容器中即@Bean

​	3.#{}

**7.登录功能实现**

​	1.修改index.html页面,显示提示，输入信息有误，

```html
<!--只有当msg为空，则不显示消息 -->
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

​	2.LoginController类攥写，用于登陆页面跳转

```java
@Controller
public class LoginController {
    //页面请求映射
    @RequestMapping("/user/login")
    //有参请求用户名密码加一个model
    public String login(@RequestParam("username") String username, @RequestParam("password") String password, Model model){
        //具体的业务
        if(!StringUtils.isEmpty((username))&&"123456".equals(password)){
            return "redirect:/main.html";
        }else{
           //告诉用户失败
           model.addAttribute("msg","用户名或者密码错误");
           return "index";
        }

    }
}
```

​	3.在MyMvcConfig中进行配置

```java
 @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/index.html").setViewName("index");
        //设置url为main.html时,跳转到dashboard页面
        registry.addViewController("/main.html").setViewName("dashboard");

    }
```

​	4.设置重定向

![1597298355665](C:\Users\jiangjinlong\AppData\Roaming\Typora\typora-user-images\1597298355665.png)

​	5.登录拦截器

​		作用就是防止直接输入XXXXX/main.html就可以进入后台

​		5.1 在config中设置登录拦截器类

```java
public class LoginhandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //登陆成功之后，会有用户的session
        Object loginUser = request.getSession().getAttribute("loginUser");
        if(loginUser==null){
            //登录用户为空的时候，需要给出提示”没有权限，无法访问“,然后返回登陆界面，这两句话很常用
            request.setAttribute("msg","没有权限，无法访问");
            //跳转回登陆界面
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }
        else{
            return true;
        }
    }
}
```

​		5.2 然后要在MyMvcConfig配置类中配置到Bean中，重写Interceptor方法

```java
 @Override
    public void addInterceptors(InterceptorRegistry registry) {
       //设置拦截内容
        registry.addInterceptor(new LoginhandlerInterceptor()).addPathPatterns("/**").excludePathPatterns("index.html","/user/login","/","/css/*","/js/**","/img/*");
    }
```

6.增删改查

​	6.1展示员工列表
​		1.提取公共页面

​		2.列表循环展示

```html
<thead>
    <tr>
        <th>id</th>
        <th>lastname</th>
        <th>email</th>
        <th>gender</th>
        <th>department</th>
        <th>birth</th>
        <th>操作</th>
    </tr>
</thead>
<tbody>
    <tr th:each="emp:${emps}">
        <td th:text="${emp.getId()}"></td>
        <td th:text="${emp.getLastName()}"></td>
        <td th:text="${emp.getEmail()}"></td>
        <!-- 男女显示判断-->
        <td th:text="${emp.getGender()==0?'女':'男'}"></td>
        <td th:text="${emp.department.getDepartmentName()}"></td>
        <!--日期格式转换 -->
        <td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>
        <td>
            <button class="btn btn-sm btn-primary">编辑</button>
            <button class="btn btn-sm btn-danger">删除</button>
        </td>
    </tr>
</tbody>
```

​	6.2 添加员工

​		1.按钮提交

​		2.跳转到添加页面

​		3.添加员工成功

​		4.跳转回首页

​	6.3 404

## **8.如何快速搭建一个网站**

​	1.前端搞定：页面长什么样

​	2.设计数据库

​	3.前端让他能够自动运行，独立化工程

​	4.数据接口如何对接：json、对象：all in one

​	5.前后端联调测试

**注意**

​	1.有一套自己的后台模板：工作要求，推荐： http://x.xuebingsi.com/index/theme/item/10.html 

​	2.前端界面:可以使用前端框架，组合一个网站

- ​		index
- ​		about
- ​		blog
- ​		post
- ​		user

​    3.让网站能够独立运行起来



## **8.Swagger**

****

**学习目标：**

- ​	了解Swagger的作用和概念
- ​	了解前后端分离
- ​	在Spring boot中集成Swagger

**Swagger简介**

**前后端分离**

​	Vue+SpringBoot

**后端时代：**前端只用负责静态页面：html=>后端.利用模板引擎：JSP=>后端是主力

**前后端分离时代：**

- 后端：后端控制层（controller）、服务层（service）、数据访问层（dao）

- 前端：前端控制层、视图层【前端团队】

  ​	伪造后端数据，json部分，不需要后端，前端也可以运行。

## **9.Druid数据源**

​	1.从maven仓库导入

```xml
<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.23</version>
        </dependency>
```

​	2.Druid介绍

数据源，你可以认为他就是一个数据库连接池，**但是又不仅仅是**

## **10.springboot整合mybatis**

**1.新建一个项目**

**2.导入相关依赖**

​	**2.1lombok依赖**

```xml
<dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```

​	**2.2springboot所对应的mybatis依赖**

```xml
 <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.3</version>
        </dependency>
```

**3.导入数据库，连接数据库，注意链接时修改时间**

**4.在核心配置文件中配置数据库连接相关设置(熟记，必须要有的)**

```properties
spring.datasource.username=root
spring.datasource.password=960708
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&userUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
server.port=8081
```

​	在test包内的自动装配所有的datasource

```java
  @Autowired
    DataSource dataSource;
```

**5.创建实体类包（看注释）**

```java
//导入Lombok之后，不用再写get、set，有参无参构造
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;

}
```

**6.创建mapper包，用于写对应的接口操作(注意看注释)**

```java
//表示这个注解这是一个mybatis的mapper类
@Mapper
//说明这是一个dao层，被spring类
@Repository
public interface UserMapper {
    int age=18;
    List<User> searchUserList();

    User searchuserById(int id);

    int addUser(User user);

    int updateUser(User user);

    int delateUser(User user);
}
```

**7.在springboot文件中以后，所有的数据库配置文件全部写在resources的包内创建mapper包，放置所有的.xml文件。注意：有一个别名设置，字段名不一致问题，设置别名，现在都在核心配置文件中设置，整合mybatis**。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace=绑定一个对应的mapper接口-->
<mapper namespace="com.jiang.mapper.UserMapper">
    <!--查询，返回值类型需要进行别名设置，直接在核心配置文件中配置，不用再写对应的包下面即com.jiang.pojo -->
    <select id="searchUserList" resultType="User">
        select* from mybatis.user
    </select>
    <select id="searchuserById" resultType="com.jiang.pojo.User" parameterType="int">
       /*定义sql*/
       select * from mybatis.user where id = #{id};
   </select>
    <!--对象中的属性可以直接取出来-->
    <insert id="addUser" parameterType="com.jiang.pojo.User">
        insert into mybatis.user (id, name, pwd) values (#{id},#{name},#{pwd});
    </insert>

    <update id="updateUser" parameterType="com.jiang.pojo.User">
        update mybatis.user set name = #{name},pwd=#{pwd} where id=#{id} ;
    </update>

    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id};
    </delete>
</mapper>
```

```properties
#直接在配置文件中整合mybatis
mybatis.type-aliases-package=com.jiang.pojo
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```



**8.这里没写service层，直接写一个controller去调用dao层，即mapper包内的，也为dao层**

```java
//所有的controller必须要写的注解
@RestController
public class UserController {
    //调用它就直接自动装配利用@Autowired注解
    @Autowired
    private UserMapper userMapper;
    //请求，/searchUserList
    @GetMapping("/searchUserList")
    public List<User> searchUserList(){
        List<User> userList=userMapper.searchUserList();
        for (User user : userList) {
            System.out.println(user);
        }
        return userList;
    }
}
```

**9.测试，直接在java包内的那个含有main方法的类中继续测试**

10.总结：spring MVC

​	M：数据和业务（dao、service）

​	C：前后端沟通交流

​	V：前端的HTML

## **11.SpringSecurity 安全问题**

在web开发中，什么时候需要注意安全？设计之初就应该考虑这些。

过滤器、拦截器

功能性需求：否

- 漏洞：隐私泄露
- 架构确定

两种：shiro和SpringSecurity：基本一致，除了类和名字

​	作用：授权和认证

权限种类：

- 功能权限
- 访问权限
- 菜单权限
- 。。。拦截器或者过滤器之前：大量原生代码

所以框架诞生了~~~

