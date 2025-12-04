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

```
\begin{longtable}{llr}
	\caption{Índices de democracia eleitoral do Brasil (1822-2024)}\label{tab:indices_democracia_br}\\
	\toprule
	\textbf{Número de observações} & \textbf{Quantidade} & \textbf{(\%)} \\
	\midrule
	\endfirsthead
	\multicolumn{3}{c}%
	{{\bfseries \tablename\ \thetable{} -- continuação}} \\
	\toprule
	\textbf{Número de observações} & \textbf{Quantidade} & \textbf{(\%)} \\
	\midrule
	\endhead
	\midrule
	\multicolumn{3}{r}{{\footnotesize Continua na próxima página}} \\
	\endfoot
	\bottomrule
	\endlastfoot
	0.0552 - 0.148 & 22 & 10,95\\
	0.148 - 0.239 & 98 & 48,76\\
	0.239 - 0.33 & 23 & 11,44\\
	0.33 - 0.421 & 19 & 9,45\\
	0.421 - 0.513 & 1 & 0,50\\
	0.513 - 0.604 & 1 & 0,50\\
	0.604 - 0.695 & 2 & 1,00\\
	0.695 - 0.786 & 7 & 3,48\\
	0.786 - 0.877 & 28 & 13,93\\
	\midrule
	\textbf{Total} & \textbf{201} & \textbf{100,00} \\
\end{longtable}
```

## Como identificar um outlier

```
def is_outlier(data, number, k=1.5):
    q1 = data.quantile(0.25)
    q3 = data.quantile(0.75)
    iqr = q3 - q1
    lower_limit = q1 - k * iqr
    upper_limit = q3 + k * iqr
    return number < lower_limit or number > upper_limit

```

## Como saber o número idea de bins

Para calcular o número ideal de bins para a Regra de Sturges, precisamos de uma fórmula que use o número de observações (pontos de dados) no seu conjunto de dados.

A Regra de Sturges é dada pela fórmula: $$k = 1 + \log_2(N)$$

A formula em Python: 
```
from numpy import log2

def adequated_number_bins(n_items):
    return 1 + log2(n_items)
```

## Gráfico de Dispersão dos Valores Reais versus Valores Preditos

```
plt.scatter(y_test, y_pred, label='Predição vs Real')

min_val = min(min(y_test), min(y_pred))
max_val = max(max(y_test), max(y_pred))

plt.plot(
    [min_val, max_val], 
    [min_val, max_val], 
    color='red', 
    linestyle='--', 
    label='Ajuste Perfeito (R²=1)'
)

plt.title('Y Real vs Y Predito (Avaliação do R²)')
plt.xlabel('Y Real (y_test)')
plt.ylabel('Y Predito (y_pred)')
plt.legend()
plt.show()
```

## Gráfico de resíduo

```
residuo = y_test - y_pred

plt.scatter(y_pred, residuo, alpha=0.6)
plt.axhline(y=0, color='red', linestyle='-') 
plt.title('Gráfico de Resíduos')
plt.xlabel('Valores Preditos (Y_pred)')
plt.ylabel('Resíduos (Y_real - Y_pred)')
plt.show()
```

## Como descobrir o número ideal de cluster no KMeans

Utilizar a fórmula sklearn.metrics.silhouette_score() presente em [link](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html).

## Como descobrir se variáveis tem multicolinearidade

Utilizar statsmodels.stats.outliers_influence.variance_inflation_factor presente em [link](https://www.statsmodels.org/stable/generated/statsmodels.stats.outliers_influence.variance_inflation_factor.html).

## Consine similarity

Compute cosine similarity between samples in X and Y. https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html

## Como avaliar um cluster e avaliar o número ideal de cluster

usar silhouette_score do sklearn. https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html#sklearn.metrics.silhouette_score

```
scores = {}
n_cluster = range(2, 15)
for num in n_cluster:
    scores[num] = silhouette_score(df_trans, KMeans(n_clusters=num, random_state=42).fit(df_trans).labels_)

max(scores, key=scores.get)
```

Pode usar também o método do joelho:

```
distortions = []
n_cluster = range(2, 15)
for num in n_cluster:
    temp_model = KMeans(n_clusters=num, random_state=42).fit(df_trans).inertia_
    distortions.append(temp_model)

fig, ax = plt.subplots(figsize=(8, 4))
ax.plot(n_cluster, distortions,marker='o')
ax.grid()
ax.set_xticks(n_cluster)
ax.set_xlabel('Number of clusters')
ax.set_ylabel('Distortion')
plt.tight_layout()
plt.show()
```

## Como aplicar transformações logarítmica para lidar com outliers, estabilizar a variância e Positive Skewness (Right Skew)

Quando o dataframe só tem valores positivod: np.log(X)

Quando o dataframe tem zeros: np.log1p(X)

Se beneficiam: 

* Modelos lineares e outros relacionados (regressão linear simples e múltipla, Ridge e Lasso, ANOVA, t/Z-test)
* Regressçao logística
* KNN
* KMeans
* PCA
* Neural Networks (Deep Learning)

O log só faria sentido se tivesse distribuições muito enviesadas (skew > 1) ou variáveis com magnitudes muito diferentes.

## Como mostra tudo em polars

```
pl.Config.set_tbl_rows(-1)  
pl.Config.set_tbl_cols(-1) 
````