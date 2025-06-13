## 🔍 对比总结：Filter vs Interceptor

| 特性            | Filter                 | Interceptor          |
| ------------- | ---------------------- | -------------------- |
| 所属规范          | Servlet API            | Spring MVC           |
| 拦截范围          | 所有请求（包括静态资源）           | 仅 Controller 层请求     |
| 触发时机          | DispatcherServlet 之前   | DispatcherServlet 之后 |
| 核心接口          | `javax.servlet.Filter` | `HandlerInterceptor` |
| 常见用途          | 安全认证、编码、日志             | 权限控制、日志、参数校验等        |
| 获取 Handler 信息 | ❌ 无法获取 Controller 方法信息 | ✅ 可拿到方法、注解等元信息       |
| 可否修改响应        | ✅ 可以                   | ⚠️ 可控性低，不适合处理响应流     |


## 事务
REQUIRED、REQUIRES_NEW、MANDATORY
有加入，没有新建
NEW 使用新的 有挂起创建，没有创建

SUPPORTS、NOT_SUPPORTED、NEVER
SUPORT 有加入，没有非事务运行
NOT SUPORT 有挂起，非事务运行

## AOP

获取代理对象，调用方法，增强代码逻辑

`@Async`
`@Transational`

```java

// 方法一
@Autowired
private ApplicationContext applicationContext;

public void outerMethod() {
    MyService proxy = applicationContext.getBean(MyService.class);
    proxy.transactionalMethod();  // ✅ 会开启事务
}


//方法二，只能方法内调用
//需要注解
@EnableAspectJAutoProxy(exposeProxy = true) // 必须加上这个
public class AppConfig {}

public void outerMethod() {
    MyService proxy = (MyService)AopContext.currentProxy();
    proxy.transactionalMethod();  // ✅ 会开启事务
}

```


## 异步

@Async
本质是通过动态代理，@Async方法被拦截将方法执行封装为异步任务提交给线程池

避免异步失效，方法内通过 this 调用

启动 @EnableAsync

默认尝试获取线程池，如果拿不到创建 SimpleAsyncTaskExecutor，对于每个请求都会启动新线程，可能会导致资源耗尽风险。建议自定义线程池。

```java

// 定义
@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean(name = "executor1")
    public Executor executor1() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(3);
        executor.setMaxPoolSize(5);
        executor.setQueueCapacity(50);
        executor.setThreadNamePrefix("AsyncExecutor1-");
        executor.initialize();
        return executor;
    }

    @Bean(name = "executor2")
    public Executor executor2() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(4);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("AsyncExecutor2-");
        executor.initialize();
        return executor;
    }
}

// 使用
@Service
public class AsyncService {

    @Async("executor1")
    public void performTask1() {
        // 任务1的逻辑
        System.out.println("Executing Task1 with Executor1");
    }

    @Async("executor2")
    public void performTask2() {
        // 任务2的逻辑
        System.out.println("Executing Task2 with Executor2");
    }
}
```