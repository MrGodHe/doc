官方文档：https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/


# springboot 简介
    轻量级java开发框架

# 注解

除了可自定义注解之外。还提供了一下通用注解，如下：

1. **条件注解 @Condition**

   - @ConditionalOnBean： 仅仅在当前上下文中存在某个对象时，才会实例化一个Bean。

   - @ConditionalOnClass：某个class位于类路径上，才会实例化一个Bean

   - @ConditionalOnExpression：当表达式为true的时候，才会实例化一个Bean

     ```
     比如：
     @ConditionalOnExpression("true")
     @ConditionalOnExpression("${my.controller.enabled:false}")
     ```

   - @ConditionalOnMissingBean：仅仅在当前上下文中不存在某个对象时，才会实例化一个Bean

   - @ConditionalOnMissingClass：某个class类路径上不存在的时候，才会实例化一个Bean

   - @ConditionalOnNotWebApplication：不是web应用

2. **@Controller 和 @RestController**

   @RestController 相当于 @Controller + @@ResponseBody; 
    该注解标识的控制类不能返回 html或者jsp等视图 

3. **@RequestMapping**

   - @GetMapping ：约束接收GET请求

   - @PostMapping：约束接收POST请求

   - 注解属性：consumes 用来限制ContentType； 
         	            produces 用来限制Accept   

     知识提示：HTTP协议Header中的两个东西 ContentType 和Accept。
             ContentType：告诉服务器当前发送的数据是什么格式
             Accept：告诉服务器，客户端能认识哪些格式,最好返回这些格式

   - 参数注解：@RequestParam(value|name|required|defaultValue) 默认required=true 	    

# 热部署

-  热部署生效

    spring.devtools.restart.enabled: true
    
    设置重启的目录
    spring.devtools.restart.additional-paths: src/main/java
    
    classpath目录下的WEB-INF文件夹内容修改不重启
    spring.devtools.restart.exclude: WEB-INF/**

# SpringMVC支持

WebMvcConfigurationSupport 这是提供MVC  Java配置的主要类。

1. 增加自定义拦截器addInterceptors
2. 扩展自定义消息转换器 extendMessageConverters，如新增fastjson的转换器来替换spring自带的MappingJackson2HttpMessageConverter
3. 处理静态资源addResourceHandlers，如（图片、js、字体、文件等）

# Spring Event事件

Spring为我们提供的一个事件监听、订阅的实现，内部实现原理是观察者设计模式，
设计初衷也是为了系统业务逻辑之间的解耦，提高可扩展性以及可维护性

**标准事件：**

1. ContextRefreshedEvent：ApplicationContext 被初始化或刷新时，该事件被发布。这也可以在 ConfigurableApplicationContext 接口中使用 refresh() 方法来发生。
2. ContextStartedEvent：当使用 ConfigurableApplicationContext 接口中的start() 方法启动 ApplicationContext 时，该事件被发布。你可以调查你的数据库，或者你可以在接受到这个事件后重启任何停止的应用程序。
3. ContextStoppedEvent：当使用 ConfigurableApplicationContext 接口中的stop() 方法停止 ApplicationContext 时，发布这个事件。你可以在接受到这个事件后做必要的清理的工作。
4. ContextClosedEvent：当使用 ConfigurableApplicationContext 接口中的close() 方法关闭 ApplicationContext 时，该事件被发布。一个已关闭的上下文到达生命周期末端；它不能被刷新或重启。
5. RequestHandledEvent：这是一个 web-specific 事件，告诉所有 bean HTTP 请求已经被服务。

**自定义事件：**

![](https://github.com/MrGodHe/doc/blob/master/image/springboot/springboot_event.png)

