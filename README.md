# haoqi
原创 一个简单的"好奇"框架 可以让你的web项目省去 Resource 或 Autowired注解  
此开源项目适合启蒙想写框架的新手 用于学习
# 使用方法
使用框架有很多种方式 按自己喜好来配置  
最终只需要将ApplicationContext交给Haoqi框架  
并且将项目的包告知Haoqi框架
项目全包名 例：cn.jiuchen.haoqi

# SpringBoot项目示例
启动类主方法里写  
```
HaoqiConfig.load(SpringApplication.run(启动类类名.class, args),"项目全包名")
```
# Maven项目示例
web.xml中将所有spring配置文件交给父级web容器  
```
<context-param>  
    <param-name>contextConfigLocation</param-name>  
    <param-value>classpath:你的spring配置文件</param-value>  
</context-param>  
<listener>  
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>  
</listener> 

<servlet>  
    <servlet-name>dispatcherServlet</servlet-name>  
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
    <init-param>  
        <param-name>contextConfigLocation</param-name>  
        <param-value></param-value> <!--这里为空不写-->  
    </init-param>  
    <load-on-startup>1</load-on-startup>  
</servlet>  
<servlet-mapping>  
    <servlet-name>dispatcherServlet</servlet-name> 
    <url-pattern>/</url-pattern>  
</servlet-mapping> 
```
写一个类实现ApplicationContextAware接口
项目中的spring配置文件需要扫描这个类所在的包（创建bean到spring容器  ）
```
@Service  //必要的注解 可以是任何能创建bean到spring容器的注解 
public class HaoqiService extends HaoqiConfig implements ApplicationContextAware {  
    @Override  
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {  
        load(applicationContext,"项目全包名");  //调用方法 交给Haoqi来管理  
    }   
}  
``` 

# 最终效果
```
   @RestController
   @RequestMapping("/api")
   public class AdminController {
       /**
            不再需要配置@Resource或@Autowired
       */
       AdminService adminService;
       UserService userService;
       ReviewService reviewService;
       CollectService collectService;
       
       @RequestMapping("/admin/list")
       public Object list(){
           return adminService.list(); //直接使用
       }
    }
```
