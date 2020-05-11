# springcloud
基于springcloud搭建的一套微服务项目

坑：

1：由于之前没有系统学习过lombok所以在使用lombok的 @Slf4j 注解的时候发现  log调用失败，后来查询发现lombok单独在pom里头引用还不够，还需要 在idea的首选项里头找到Plugins里头搜索 lombok点击安装之后就没有问题了，
   因为引用lombok之后我们可以用
   @AllArgsConstructor
   @NoArgsConstructor
   添加全参构造函数已经无参构造，如果lombok引用不成功这俩个构造不会出现，
2:我的项目最初并没有引用什么euraka但是却报错链接不上，原因是我项目最初拷贝了  其他作者的pom，里头包含了euraka的东西，即使后来删除了，但是在扩展插件里头还是有，参考如下文章重新更新maven  https://www.jianshu.com/p/1b9549f3aac4



技术解析：
 1：RestTemplate 这是一个  替代httpclient的  Rest风格的  HTTP访问框架
 2: Eureka 服务注册中心  用户搭建服务集群  相同技术有zoonkeeper consul 还有阿里的nacos  相比较 zookepper/consul等相同框架  Eureka属于AP类型的集群框架，也就是当被监控的服务没有心跳之后不会立马被踢出集群，会等待他自己回来，这样能保证服务的可用性，但是无法保证数据的一致性，像zookepper,consul属于cp类型的，当服务失去心跳后会立马踢出集群，保证数据的高一致性。当然Eureka还可以设置服务被踢出去的时间以及机制，默认是AP类型的产品。
 3：Ribbon当客户端借助RestTemplate（@LoadBrance  支持负载均衡）去访问Eureka下面的集群服务是，Eureka有自身的   轮训负载均衡机制但是这还是不够的，所以后续要引进Ribbon解决负载均衡的算法，Ribbon结合RestTemplate 调用
 4：我们已经知道Ribbon是一个负载均衡集合RestTemplate的框架，用RestTemplate框架调用的时候需要添加@loadblance注解 等才能完成调用  Feign是集成了Ribbon+RestTemplate，简化了Ribbon的使用,可以实现超时设置，日志增强
 5：当服务端出现故障的时候客户端可能就会卡死，这样会导致连接沾满导致雪崩问题，这个时候，我们可能就需要一套框架用户服务熔断以及降级，兜底方案，提高体验，这就是hystryx(服务降级 fallback 服务熔断（就是防止自己被压死，自我保护开启直接停止服务然后抛出友好提示）break 服务限流 flowlimit)
