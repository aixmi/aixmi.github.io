### spring boot /spring cloud 配置
#### 数据源配置(todo: 每一个配置的解释，以及如何配置最佳)
spring:
  application:
    name: spring-boot-layui-admin-seed-project
  datasource:
    hikari:
      auto-commit: true
      connection-test-query: SELECT 1
      connection-timeout: 10000
      idle-timeout: 30000
      max-lifetime: 900000
      maximum-pool-size: 30
      minimum-idle: 10
      pool-name: HikariCP
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: ''
    type: com.zaxxer.hikari.HikariDataSource
    url: jdbc:mysql://<your-database-host>:<your-database-port>/<your-database-name>?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai