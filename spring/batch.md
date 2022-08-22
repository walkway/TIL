# Batch Insert

````
spring:
    jpa:
        database: mysql
        properties:
            hibernate.jdbc.batch_size: 100
            hibernate.order_inserts: true
            hibernate.order_updates: true
            hibernate.dialect: org.hibernate.dialect.MySQL5InnoDBDialect
            hibernate.show_sql: true

    datasource:
        url: jdbc:mysql://localhost:3366/test_db?useSSL=false&serverTimezone=UTC&autoReconnect=true&rewriteBatchedStatements=true
        driver-class-name: com.mysql.cj.jdbc.Driver
````

### batch_size
한 번에 insert 되는 rows

### rewriteBatchedStatements
````
MySQL Connector/J 8.0 Developer Guide : 6.3.13 Performance Extensions
Stops checking if every INSERT statement contains the “ON DUPLICATE KEY UPDATE” clause. As a side effect, obtaining the statement’s generated keys information will return a list where normally it wouldn’t. Also be aware that, in this case, the list of generated keys returned may not be accurate. The effect of this property is canceled if set simultaneously with ‘rewriteBatchedStatements=true’.
````

### Session batching
- https://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/Hibernate_User_Guide.html#batch-session-batch
- @GeneratedValue(strategy = GenerationType.IDENTITY)Batch Insert 지원하지 않음
