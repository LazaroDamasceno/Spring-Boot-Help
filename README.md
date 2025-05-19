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
package com.api.v1.borrow;

import org.junit.jupiter.api.MethodOrderer;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestMethodOrder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.reactive.server.WebTestClient;

@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ManyBorrowsTest {

    @Autowired
    private WebTestClient webTestClient;

    @Test
    @Order(1)
    void testSuccessfulManyBookBorrowings() {

        int testcases = 10;

        while(testcases > 0) {
            String ssn = "123456789";
            String isbn = "123456789012";
            String endpoint = "api/v1/borrows/%s/%s".formatted(ssn, isbn);

            webTestClient
                    .post()
                    .uri(endpoint)
                    .exchange()
                    .expectStatus()
                    .is2xxSuccessful();

            testcases--;
        }


    }

    @Test
    @Order(2)
    void testUnsuccessfulBookBorrowing1() {

        String ssn = "123456788";
        String isbn = "123456789012";
        String endpoint = "api/v1/borrows/%s/%s".formatted(ssn, isbn);

        webTestClient
                .post()
                .uri(endpoint)
                .exchange()
                .expectStatus()
                .is5xxServerError();
    }

    @Test
    void testUnsuccessfulBookBorrowing2() {

        String ssn = "123456789";
        String isbn = "123456789011";
        String endpoint = "api/v1/borrows/%s/%s".formatted(ssn, isbn);

        webTestClient
                .post()
                .uri(endpoint)
                .exchange()
                .expectStatus()
                .is5xxServerError();
    }

    @Test
    void testUnsuccessfulBookBorrowing3() {

        String ssn = "123456789";
        String isbn = "123456789012";
        String endpoint = "api/v1/borrows/%s/%s".formatted(ssn, isbn);

        webTestClient
                .put()
                .uri(endpoint)
                .exchange()
                .expectStatus()
                .is4xxClientError();
    }

}
```

## MVC test

```
@AutoConfigureMockMvc
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class CustomerRegistrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private ObjectMapper objectMapper;

    CustomerRegistrationDto registrationDto = new CustomerRegistrationDto(
            new PersonRegistrationDto(
                    "Leo",
                    "",
                    "Santos",
                    LocalDate.parse("2000-12-12"),
                    "123456789",
                    "leosantos@mail.com",
                    "1234567890",
                    Gender.CIS_MALE
            ),
            new Address(
                    "CA",
                    "LA",
                    "Downtown",
                    "90012"
            )
    );

    @Test
    @Order(1)
    void testSuccessfulRegistration() throws Exception {
        mockMvc.perform(MockMvcRequestBuilders
                .post("/api/v2/customers")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(registrationDto))
        ).andExpect(status().is2xxSuccessful());
    }
```

## Reactive Postgres

```
spring.r2dbc.url=r2dbc:postgresql://localhost:5432/postgres
spring.r2dbc.username=postgres
spring.r2dbc.password=postgres

spring.flyway.enabled=true
spring.flyway.url=jdbc:postgresql://localhost:5432/postgres
spring.flyway.user=postgres
spring.flyway.password=postgres
```

## Java Offset Hours

```
import java.time.Instant;
import java.time.ZoneId;

public class Main {
    public static void main(String[] args) {
        int offsetHours = ZoneId
                .of(ZoneId.systemDefault().toString())
                .getRules()
                .getOffset(Instant.now())
                .getTotalSeconds() / 3600;
        System.out.println(offsetHours);
    }
}
```

## Global Exception Handler

```
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(DuplicatedSsnException.class)
    public ResponseEntity<String> handleException(DuplicatedSsnException ex) {
        return ResponseEntity.status(HttpStatus.CONFLICT).body(ex.getMessage());
    }
}
```
## R

```
data_long <- pivot_longer(data, cols = c(column1, column2), 
                          names_to = "", values_to = "")
```
```
geom_bar(stat="identity", position="dodge")
```
```
  geom_text(
    aes(label = paste0(Percentage, "%")),
    position = position_dodge(width = 0.9),
    hjust = -0.1,
    size = 3
  )
```
```
library(ggplot2)
library(tidyr)

my_data <- data.frame(
  Objects = c('A', 'B', 'C'),
  P2000 = c(30,40,50),
  P2010 = c(50,60,70),
  P2020 = c(60,70,80)
)


data_long = pivot_longer(
  my_data,
  cols = c(P2000, P2010, P2020),
  names_to = "Year",
  values_to = "Percentage"
)

ggplot(
    data_long, 
    aes(Objects, Percentage, fill = Year)
  ) +
  geom_bar(
    stat = "identity",
    position = "dodge"
  ) +
  geom_text(
    aes(label = paste0(Percentage, "%")),
    position = position_dodge(width = 0.9),
    hjust = -0.1,
    size = 3
  ) +
  coord_flip() +
  labs(
    title = "Title",
    x = "Y",
    y = "%",
    fill = "Year",
    caption = "Caption",
    subtitle = "Subtitle"
  ) +
  scale_fill_discrete(labels = c(2000, 2010, 2020)) +
  ylim(0, 100)
  theme_gray()
```
