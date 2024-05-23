Alibaba nacos使用derby数据库存在sql注入

Nacos 支持多种数据库，包括 Derby、MySQL、MariaDB、PostgreSQL、Oracle 和 SQL Server 等。其中，Derby 是 Nacos 默认的嵌入式数据库，而其他数据库需要手动配置才能使用。

漏洞地址： http://nacos.xxxxxxx.com.ph:8848/nacos/v1/cs/ops/derby?sql=select%20*%20from%20users

漏洞描述： Alibaba nacos使用derby数据库存在sql注入,可直接执行sql查询相关敏感信息。

漏洞详情：

1，如果使用的是derby数据库，访问上述接口。 image

2，执行sql语句，得到相关敏感数据。 image

这个查询的数据输出表明了对 SQL 查询的直接执行，而没有对用户输入的 SQL 进行任何过滤或者防护。

3，如果不是使用的derby数据库，则提示 "The current storage mode is not Derby" image

漏洞版本： nacos企业版 2.0.3,2.1.2 image image

derby数据库版本 10.14.2.0 image

修复建议：

1,为了防止 SQL 注入攻击，应该采取参数化查询等安全措施来处理用户输入的 SQL 查询。

2,使用手动配置数据库，在 Nacos 的配置文件中，可以通过属性来指定使用的数据库类型，例如：application.propertiesspring.datasource.platform spring.datasource.platform=mysql

同时，还需要配置数据库的连接信息，例如： spring.datasource.url=jdbc:mysql://localhost:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true

spring.datasource.username=root

spring.datasource.password=xxxxxxx

以上示例配置了使用 MySQL 数据库，并指定了连接信息。
