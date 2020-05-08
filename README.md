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
 2: Eureka 服务注册中心  用户搭建服务集群  相同技术有zoonkeeper consul 还有阿里的nacos
 3：Ribbon当客户端借助RestTemplate（@LoadBrance  支持负载均衡）去访问Eureka下面的集群服务是，Eureka有自身的   轮训负载均衡机制但是这还是不够的，所以后续要引进Ribbon解决负载均衡的算法
