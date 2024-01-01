# POM-Templates

## H2 Database

spring.datasource.url=jdbc:h2:mem:app<br>
spring.datasource.driver-class-name=org.h2.Driver<br>
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect<br>
spring.datasource.username=sa<br>
spring.h2.console.path=/api<br>
spring.h2.console.enabled=true<br>
spring.jpa.defer-datasource-initialization=true<br>
spring.jpa.hibernate.ddl-auto=update<br>
spring.jpa.properties.hibertane.jdbc.lob.non_contextual_creation=true<br>

## PostgreSQL

spring.jpa.database=postgresql<br>
spring.sql.init.platform=postgresql<br>
spring.datasource.driver-class-name=org.postgresql.Driver<br>
spring.datasource.url=jdbc:postgresql://<br>
spring.datasource.username=<br>
spring.datasource.password=<br>
spring.datasource.hikari.maximum-pool-size=2<br>
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect<br>
spring.jpa.hibernate.ddl-auto=update<br>
