# Aplication.property

## H2 

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
```

## Customized annotation

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
@NotNull
@Size(min = 9, max = 9)
public @interface SSN {
  String message() default "Invalid SSN format. Please enter a 9-digit Social Security Number.";
} 
```

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
@NotNull
@Size(min = 18, max = 18)
@Pattern(regexp = "^([0-9]{2})/([0-9]{2})/([0-9]{4})$")
public @interface DateFormat {
  String message() default "Invalid date time format. Please enter date time as dd/mm/yyyy hh:mm:ss.";
}
```

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
@NotNull
@Size(min = 18, max = 18)
@Pattern(regexp = "^([0-9]{2})/([0-9]{2})/([0-9]{4}) ([0-1][0-9]|2[0-3]):([0-5][0-9]):([0-5][0-9])$")
public @interface DateTimeFormat {
  String message() default "Invalid date time format. Please enter date time as dd/mm/yyyy hh:mm:ss.";
}
```

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
@NotNull
@Size(min = 18, max = 18)
@Pattern(regexp = "^([0-9]{2})-([0-9]{2})-([0-9]{4}) ([0-1][0-9]|2[0-3]):([0-5][0-9]):([0-5][0-9])$")
public @interface DateTimeFormatForGET {
  String message() default "Invalid date time format. Please enter date time as dd/mm/yyyy hh:mm:ss.";
}
```

## 1:N

```
@ManyToOne(cascade = CascadeType.ALL)
@JoinColumn(name = "physician_id")
@JsonBackReference
private Physician physician;
```

```
@OneToMany(cascade = CascadeType.ALL, fetch = FetchType.LAZY, mappedBy = "physician")
@JsonManagedReference
private List<MedicalAppointment> appointmentList;
```

## MySQL

```
spring.datasource.url=jdbc:mysql://localhost:3306/[db_name]
spring.datasource.username=[db_user_name]
spring.datasource.password=[db_password]
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
```

## PostgreSQL

```
spring.jpa.database=postgresql
spring.sql.init.platform=postgresql
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://
spring.datasource.username=
spring.datasource.password=
spring.datasource.hikari.maximum-pool-size=2
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
```

Use the setting above with [elephantsql](https://www.elephantsql.com/).

# POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.2.4</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.api</groupId>
	<artifactId>v1</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>v1</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>17</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<excludes>
						<exclude>
							<groupId>org.projectlombok</groupId>
							<artifactId>lombok</artifactId>
						</exclude>
					</excludes>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>
```

