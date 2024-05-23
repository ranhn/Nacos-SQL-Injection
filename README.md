Alibaba Nacos using Derby database with SQL injection

Nacos supports multiple databases, including Derby MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server, among others. Among them, Derby is the default embedded database for Nacos, while other databases require manual configuration to be used.

Vulnerability Address： http://nacos.xxxxxxx.com.ph:8848/nacos/v1/cs/ops/derby?sql=select%20*%20from%20users

Vulnerability Description： Alibaba Nacos uses the derby database for SQL injection, which allows for direct execution of SQL queries for sensitive information.

Vulnerability details：

1，If using a derby database, access the above interface 
![image](https://github.com/ranhn/Nacos-SQL-Injection/assets/107679328/731a6eb5-092b-43d6-b130-3dc32e33e142)


2，Execute SQL statements to obtain relevant sensitive data. 
![image](https://github.com/ranhn/Nacos-SQL-Injection/assets/107679328/576ab611-9ab0-4a20-9832-59518fc093bb)


The data output of this query indicates direct execution of the SQL query without any filtering or protection on the SQL input by the user.

3，If not using a derby database, prompt "The current storage mode is not Derby" 
![image](https://github.com/ranhn/Nacos-SQL-Injection/assets/107679328/33464fe3-4719-40b4-9316-26fbd16082d8)


Vulnerable versions: nacos enterprise version 2.0.3 and 2.1.2
![image](https://github.com/ranhn/Nacos-SQL-Injection/assets/107679328/b8ebcc5c-0e23-4d51-b826-95c46af84bd3)

![image](https://github.com/ranhn/Nacos-SQL-Injection/assets/107679328/2794a887-0dc8-4069-856c-4417701200ae)

Derby database version 10.14.2.0 

![image](https://github.com/ranhn/Nacos-SQL-Injection/assets/107679328/f351c1bc-890e-4267-8c4a-e2c83486044c)

Repair suggestions：

1,To prevent SQL injection attacks, security measures such as parameterized queries should be taken to handle user input SQL queries.

2,Using manual database configuration, in the Nacos configuration file, the type of database used can be specified through properties, such as：application.propertiesspring.datasource.platform spring.datasource.platform=mysql

At the same time, it is necessary to configure the connection information of the database, such as： spring.datasource.url=jdbc:mysql://localhost:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true

spring.datasource.username=root

spring.datasource.password=xxxxxxx

The above example configured the use of MySQL database and specified connection information.
