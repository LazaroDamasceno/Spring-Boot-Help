# Aplication.property

## Serializable

```
public class SystemUser implements Serializable {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private UUID id;
```

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

# Reactive MongoDB

`spring.data.mongodb.uri=mongodb://localhost:27017/demo`

# Reactive test

```
package com.api.v1;

import com.api.v1.dtos.requests.NewCustomerRequestDto;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.reactive.server.WebTestClient;

import java.time.LocalDate;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class CustomerTests {

	@Autowired
	private WebTestClient webTestClient;

	@Test
	void testSuccessfulCustomerRegistration() {

		var newCustomer = new NewCustomerRequestDto(
				"Leo",
				"",
				"Santos",
				"123456789",
				LocalDate.parse("2000-12-12"),
				"leosantos@mail.com",
				"St. Dennis, Paris",
				"1234567890",
				"male"
		);

		webTestClient
				.post()
				.uri("api/v1/customers")
				.bodyValue(newCustomer)
				.exchange()
				.expectStatus().is2xxSuccessful();
	}

	@Test
	void testUnsuccessfulCustomerRegistration() {

		var newCustomer = new NewCustomerRequestDto(
				"Leo",
				"",
				"Santos",
				"123456789",
				LocalDate.parse("2000-12-12"),
				"leosantos@mail.com",
				"St. Dennis, Paris",
				"1234567890",
				"male"
		);

		webTestClient
				.post()
				.uri("api/v1/customers")
				.bodyValue(newCustomer)
				.exchange()
				.expectStatus().is5xxServerError();
	}

	@Test
	void testSuccessfulCustomerUpdate() {

		var newCustomer = new NewCustomerRequestDto(
				"Leo",
				"Silva",
				"Santos Jr",
				"123456789",
				LocalDate.parse("2000-12-12"),
				"jr@leosantos.com",
				"St. Dennis, Paris, Europe",
				"0987654321",
				"cis male"
		);

		webTestClient
				.put()
				.uri("api/v1/customers")
				.bodyValue(newCustomer)
				.exchange()
				.expectStatus().is2xxSuccessful();
	}

	@Test
	void testUnsuccessfulCustomerUpdate() {

		var newCustomer = new NewCustomerRequestDto(
				"Leo",
				"",
				"Santos Jr",
				"123456788",
				LocalDate.parse("2000-12-12"),
				"jr@leosantos.com",
				"St. Dennis, Paris, Europe",
				"0987654321",
				"cis male"
		);

		webTestClient
				.put()
				.uri("api/v1/customers")
				.bodyValue(newCustomer)
				.exchange()
				.expectStatus().is5xxServerError();
	}

	@Test
	void testSuccessfulAllCustomersDeletion() {
		webTestClient
				.delete()
				.uri("api/v1/customers")
				.exchange()
				.expectStatus()
				.is2xxSuccessful();

	}

	@Test
	void testUnsuccessfulAllCustomersDeletion() {
		webTestClient
				.delete()
				.uri("api/v1/customers")
				.exchange()
				.expectStatus()
				.is5xxServerError();
	}

}
```
