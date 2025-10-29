# Aplication.property

## Serializable

```
public class SystemUser implements Serializable {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private UUID id;
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
# MongoDB

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

## Virtual Thread

```
spring.threads.virtual.enabled=true
```

## Python: formatação númerica BR com GridSpec

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.gridspec import GridSpec
import matplotlib.ticker as ticker
import locale

locale.setlocale(locale.LC_ALL, 'pt_BR.UTF-8')

def format_brazilian(*args):
    return locale.currency(args[0], grouping=True, symbol=None)

fig = plt.figure(figsize=(12, 8))

gs = GridSpec(3, 2, figure=fig)

ax1 = fig.add_subplot(gs[0, :])
ax2 = fig.add_subplot(gs[1, :])
ax3 = fig.add_subplot(gs[2, :])

sns.lineplot(df_bancos, x='ano', y='valor_incentivo', hue='nome_grupo', ax=ax1, errorbar=None)
sns.lineplot(df_energia, x='ano', y='valor_incentivo', hue='nome_grupo', ax=ax2, errorbar=None)
sns.lineplot(df_comunicacao, x='ano', y='valor_incentivo', hue='nome_grupo', ax=ax3, errorbar=None)

titulos = ['Bancos', 'Energia', 'Comunicação']
axes = [ax1, ax2, ax3]
anos = df['ano'].unique().tolist()
formatter = ticker.FuncFormatter(format_brazilian)
for ax, titulo in zip(axes, titulos):
    ax.set_title(titulo)
    ax.set_xlabel('Ano')
    ax.legend(title=None)
    ax.set_xticks(anos)
    ax.tick_params('x', rotation=90)
    ax.yaxis.set_major_formatter(formatter)

for ax in [ax2, ax3]:
    ax.set_ylabel(None)

for ax in [ax1, ax2]:
    ax.set_ylabel('Valor do incentivo (R$)')

ax1.legend(ncols=4)

plt.tight_layout()
fig.savefig('linha_historico_investimento_estatais', dpi=300, bbox_inches='tight')
plt.show()
```

## Adicionar % no mapa coroplético

```
LIMIAR_PARA_TEXTO_CLARO = mapa['pct_valor_incentivado'].mean() 

for idx, row in mapa.iterrows():
    valor = row['pct_valor_incentivado']
    label = f"{valor:.2f}%" 

    if valor > LIMIAR_PARA_TEXTO_CLARO:
        text_color = 'black'
    else:
        text_color = 'white'
        
    ax.annotate(
        text=label,
        xy=row['coords'], 
        horizontalalignment='center',
        fontsize=6,
        color=text_color,
        weight='bold'
    )
```

## Latex Table

```
\begin{table}[H]
	\centering
	\caption{}
	\label{tab:}
	\begin{tabular}{lc} 
		\toprule
		Unidade Federativa & (\%) \\ 
		\midrule 
		Paraná & 35,17\% \\ 
		Rio Grande do Sul & 38,01\% \\ 
		Santa Catarina & 26,02\% \\ 
		Total & 100,00\% \\ 
		\bottomrule 
	\end{tabular}
	\par
	\footnotesize{Fonte: elaboração baseada em \cite{salicnet}.}
\end{table}
```