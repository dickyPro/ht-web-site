# fast-cache-starter
一款轻量级开源缓存系统，支持多种L1,L2级缓存自由组合、切换，L1缓存使用内存（可选择guava、Caffeine），L2缓存使用 Redis(推荐)/Memcached等，可以由用户自由拓展。<br>
<br>
基于spring boot上的注解缓存，自带轻量级缓存管理页面，采用gson序列化与反序列化，以json串存于缓存之中，支持单个缓存设置过期时间，可以根据前缀移除缓存。fast-cache-starter可以快速用于日常的spring boot/spring mvc应用之中。<br>
 
# 应用场景

* 1.系统原先无缓存情况下，使用cache可快速提升性能，避免应用重启导致的缓存冷启动后对后端业务的冲击。
* 2.与redis /Memcached 等外置缓存解耦，L1的存在可降低对L2的读取次数，即使断开L2外置缓存，系统仍可正常运行并且保持一定缓存效率；
 
# 缓存结构
Cache 的两级缓存结构<br>
+ L1： 进程内缓存
+ L2： Redis/Memcached 外置缓存
+ 读取顺序：L1->L2->database

# 使用手册
### Maven依赖
```
<dependency>
    <groupId>com.github.franky</groupId>
    <artifactId>fast-cache-starter</artifactId>
    <version>1.0.0</version>
</dependency>
```
### 缓存配置
例如配置redis，在application.yml文件中配置：
```
#redis-cache 相关
redis:
    pool:
         maxActive: 300
         maxIdle: 100
         maxWait: 1000
    host: 127.0.0.1
    port: 6379
    password:
    timeout: 2000
    # 服务或应用名
    sysName: ace
    enable: true
    database: 0
```
	
### 缓存开启
@EnableFastCache

### 使用缓存

在方法上进行@Cacheable注解或@CacheClear注解
```
	/**
	 * 缓存注解
	 *
	 * KEY写法：
	 * 不适配参数：@WechatCacheable(key = "{WechatPlatform}.ht:getEhrUsers")
	 * 适配参数：@WechatCacheable(key = "'{WechatPlatform}.ht:getEhrUsers'+#USERID")
	 */
```
 
### 轻量管理端
访问地址：http://localhost:8080/cache ;<br>
管理端批量或前缀清除fast-cache-starter注册的缓存，同时也可以快速预览缓存的数据内容，也可以对缓存的失效时间进行延长。 <br>

# 兼容spring mvc模式
### 配置文件 
+ application.properties
```
redis.pool.maxActive = 300
redis.pool.maxIdle = 100
redis.pool.maxWait = 1000
redis.host = 127.0.0.1
redis.port = 6379
redis.password = 
redis.timeout = 2000
redis.database = 0
redis.sysName = ace
redis.enable = true
```
+ applicationContext.xml
```
<!-- beans 头部-->
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
	http://www.springframework.org/schema/context  
	http://www.springframework.org/schema/context/spring-context-3.0.xsd"	
<!-- 开启AOP配置 -->	
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
<context:component-scan base-package="com.ht.cache"/>
<context:annotation-config/> 
```
### maven依赖
```
<properties>
    <!-- spring -->
    <spring.version>4.1.3.RELEASE</spring.version>
<properties>
<dependencies>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-core</artifactId>
    	<version>${spring.version}</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-beans</artifactId>
    	<version>${spring.version}</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-context</artifactId>
    	<version>${spring.version}</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-context-support</artifactId>
    	<version>${spring.version}</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-aspects</artifactId>
    	<version>${spring.version}</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-webmvc</artifactId>
    	<version>${spring.version}</version>
    </dependency>
    <dependency>
    	<groupId>org.aspectj</groupId>
    	<artifactId>aspectjrt</artifactId>
    	<version>${aspectj.version}</version>
    </dependency>
</dependencies>
```
### 使用方式
使用方式与spring boot的方式一样，在方法上直接注解即可。

# 项目相关
+ 贡献者 Franky<br>
+ 未完成部分：
++ 缓存更新cachePut实现；
++ 集成 memcache；
++ 审批管理端功能；

