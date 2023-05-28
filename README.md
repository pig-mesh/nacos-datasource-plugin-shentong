Nacos 从 2.2.0 版本开始,可通过 SPI 机制注入多数据源实现插件,并在引入对应数据源实现后,便可在 Nacos 启动时通过读取
application.properties 配置文件中 spring.datasource.platform 配置项选择加载对应多数据源插件.

![Nacos 插件化实现
](https://minio.pigx.vip/oss/202212/1671179590.jpg)

> Nacos 官方默认实现 MySQL、Derby ，其他类型数据库接入需要参考下文自己扩展。

![](https://minio.pigx.vip/oss/202212/1671180565.png)

## 自定义 shentong 插件

### 1. 添加 shentong 插件

> 依赖已上传 maven 中央仓库，请勿使用阿里云代理

```xml
<dependency>
    <groupId>com.pig4cloud.plugin</groupId>
    <artifactId>nacos-datasource-plugin-shentong</artifactId>
    <version>0.0.1</version>
</dependency>

<dependency>
    <groupId>com.stdb</groupId>
    <artifactId>stoscarJDBC</artifactId>
    <version>16</version>
    <scope>system</scope>
    <systemPath>/Users/lengleng/Downloads/oscarJDBC16.jar</systemPath>
</dependency>
```

### 2. 获取神通数据库脚本

使用神通数据库提供的数据迁移工具可以将Nacos官方提供的MySQL脚本准确地转换为神通的数据库和对象存储。

![](https://minio.pigx.vip/oss/202305/1685288249.png)

### 3. 配置 nacos 数据源链接信息

```yaml
db:
  num: 1
  user: SYSDBA
  password: szoscar55
  url: jdbc:oscar://172.16.1.200:2003/OSRDB?currentSchema=PIG_CONFIG
  pool.config.driver-class-name: com.oscar.Driver
  pool.config.connection-test-query: 'SELECT 1 FROM DUAL'
```

### 4. 指定 nacos 数据源平台

```yaml
spring:
  datasource:
    platform: shentong
```

![](https://minio.pigx.vip/oss/202305/1685288129.png)
