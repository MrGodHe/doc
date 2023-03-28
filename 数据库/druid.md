# Druid是什么

Druid 是一个 JDBC 组件库，包含数据库连接池、SQL Parser 等组件

| 文档地址 | https://github.com/alibaba/druid |
| -------- | -------------------------------- |

# Druid 如何使用

- 单独使用

  ```
  <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.1.12</version>
  </dependency>
  ```

- 结合springboot

  ```
  <dependency>
     <groupId>com.alibaba</groupId>
     <artifactId>druid-spring-boot-starter</artifactId>
     <version>1.1.17</version>
  </dependency>
  ```

  

# druid连接池