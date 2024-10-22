# OpenFeign 浅述


<!--more-->

## 引言

现在我们在微服务开发中，由于服务拆分，我们不可避免的会涉及一个服务需要调用另一个服务的 RPC 场景，对于这一场景的实现有很多方案：我们可以自己封装`httpClient`，也可以使用`RestTemplate`或者`Dubbo`，以及我本文要讲的`OpenFeign`，这些都可以方便我们来完成远程调用。

<br>

## 示例

假设我们有一个场景：

> 我们有两个服务：认证权限服务`auth-service`，还有一个应用服务`app-service`，应用服务登入的时候需要调用认证权限服务来进行账号认证和权限校验。

`auth-service`中有一个认证接口：

```java
@RestController
@RequestMapping("/api)
public class AuthController {

    @GetMapping("/auth")
    public Boolean auth(@RequestBody AuthRequest authRequest){
    // 认证逻辑 ...
        return true;
    }
}
```

而由于我们在`app-service`中需要去调用`auth-service`的auth接口，所以我们可以使用`OpenFeign`来帮我们完成RPC的过程，实现步骤如下：

1. 首先需要引入maven依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

<br>

2. 使用`@EnableFeignClients`注解来启用`OpenFeign`的能力,比如可以在`app-service`启动项`AppServiceStater`上标注这个注解：

```java
@EnableFeignClients
@SpringBootApplication
public class AppServiceStater {
    public static void main(String[] args) {
        SpringApplication.run(ServiceProviderAppStater.class, args);
    }
}
```

<br>

3. 以接口的方式构建我们需要RPC操作的远程接口配置

比如在目前场景中，我们需要操作认证服务`auth-service`的`/api/auth`接口，那么我们可以如下操作：

```java
@FeignClient(name = "auth-service",url = "http://127.0.0.1:8080")
public interface AuthServiceRemoteClient {

@GetMapping(value = "/api/auth")
Boolean auth(@RequestBody AuthRequest);
}
```

<br>

4. 注入3中写的接口并调用方法

```java
@RestController
public class NacosController {

    @Autowired
    AuthServiceRemoteClient authServiceRemoteClient;

    @GetMapping("/app/login")
    public Boolean login(@RequestBody LoginRequest loginRequest){
        // 转换为AuthRequest auth
        AuthRequest authRequest = Convert(loginRequest);
        return authServiceRemoteClient.auth(authRequest);
    }
}
```

完成以上几个步骤后，当我们请求`app-servie`的login接口的时候，内部就会调用`auth-service`的auth接口进行认证，整个过程还是很丝滑的。

<br>

## 思想内核

上面的案例，相信你很想知道整个`OpenFeign`的实现是怎么样的，再了解细节之前，我们先站在上帝视角来看下这个思想内核：

### 构建过程

当项目启动的时候，`OpenFeign`会扫描指定的标注了`@FeignClient`的注解，根据上面案例我们可以知道`@FeignClient`是标注在接口之上的，扫描到这个接口后，`OpenFeign`会通过JDK动态代理的方式为这个接口生成代理对象；而接口中的每一个方法都是对应了一个远程的API接口，如何在调用指定的方法就可以调到远程的指定接口呢？

这是`OpenFeign`在解析接口时，接口中的每个方法会被解析成`MethodMeradata`信息，然后再转换成`MethodHandler`，最终解析完所有的方法会构成一个`Map<Method,MethodHandler>`对象，而这个对象会作为`InvocationHandler`的一个属性而存在，我们都知道`InvocationHandler`时JDK动态代理的一个核心组件，所有被代理的对象方法调用都会走到`InvocationHandler`的invoke方法逻辑。

### 调用执行过程

当已经构建好所有的`@FeignClient`标注接口的代理对象时，我们调用指定的方法时，会从`Map<Method,MethodHandler>`对象中根据Method来获取指定的`MethodHandler`对象，然后执行其invoke方法进行真正的RPC逻辑。

![OpenFeign构建调用过程](https://cdn.jsdelivr.net/gh/Turbo-King/images/a.jpg "OpenFeign构建调用过程")

<br>

## 总结

本篇文章不仅仅描述了`OpenFeign`的构建执行示例，以及整个JDK动态代理的实现，是在一个很高的层面来看整个`OpenFeign`的实现原理，而这一切重中之重正是在`MethodHandler`对象的invoke方法之中。期待未来我们一起探索。














