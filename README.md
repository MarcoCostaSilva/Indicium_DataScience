# 🎬 Desafio Lighthouse – Análise Cinematográfica e Previsão de Nota IMDB

## 📝 Introdução

Este projeto faz parte do **Desafio Lighthouse da Indicium**, focado na área de **Data Science**.  
O objetivo é analisar dados de filmes e fornecer insights para auxiliar na decisão de qual tipo de filme deve ser produzido a seguir, além de desenvolver um modelo preditivo para estimar a nota do IMDB. A seguir, estão descritas todas as etapas do projeto, com explicações detalhadas sobre cada fase da análise e modelagem.

---

## 🎯 Objetivos do Projeto

🔹 Explorar e entender a base de dados cinematográficos;  
🔹 Gerar insights sobre gêneros, faturamento, notas e sinopses;  
🔹 Responder perguntas específicas de negócios e análise de dados;  
🔹 Desenvolver um modelo preditivo para IMDB_Rating;  
🔹 Documentar o processo completo de forma clara e organizada;  

---

## Estrutura do Desafio:

### 1️⃣ Análise Exploratória de Dados (EDA):
- Exploração inicial das variáveis; 
- Criação de gráficos e tabelas resumidas;  
- Identificação de padrões e hipóteses iniciais;  

### 2️⃣ Respostas a perguntas estratégicas:
- Qual filme recomendar para uma pessoa que você não conhece?  
- Quais fatores influenciam o faturamento esperado de um filme?  
- O que a coluna *Overview* revela sobre o gênero ou características do filme?  

### 3️⃣ Modelagem Preditiva:
- Variáveis e transformações utilizadas; 
- Tipo de problema: **Regressão**;  
- Escolha do modelo e justificativa;  
- Métricas de avaliação; 

### 4️⃣ Previsão para um filme específico:
- Filme: *The Shawshank Redemption*;  
- Previsão da nota IMDB com o modelo treinado; 

### 5️⃣ Entregáveis finais:
- Modelo salvo em `.pkl`;  
- Notebook ou script Python com o passo a passo;  
- Relatórios de análise;  
- Arquivo `requirements.txt` com pacotes utilizados;
- README detalhado com instruções e documentação;  

---

## 📊 Dicionário de Dados:

| Coluna | Descrição |
|--------|-----------|
| Series_Title | Nome do filme |
| Released_Year | Ano de lançamento |
| Certificate | Classificação etária |
| Runtime | Duração do filme |
| Genre | Gênero do filme |
| IMDB_Rating | Nota do IMDB |
| Overview | Sinopse/resumo do filme |
| Meta_score | Média ponderada de críticas |
| Director | Diretor |
| Star1 | Ator/Atriz principal |
| Star2 | Ator/Atriz secundário |
| Star3 | Ator/Atriz terciário |
| Star4 | Ator/Atriz coadjuvante |
| No_of_Votes | Número de votos no IMDB |
| Gross | Faturamento |

## 🔎 Etapa 1 – Análise Exploratória dos Dados (EDA)

O objetivo é conhecer melhor a estrutura da tabela, verificar o tamanho do conjunto de dados, observar os tipos de variáveis e identificar se existem valores ausentes. Essa etapa é importante porque nos ajuda a entender quais informações estão disponíveis e como elas poderão ser utilizadas nas análises e modelagens seguintes.

## Carregamento dos Dados

```python
# Importando biblioteca
import pandas as pd:

# Link:
url = "https://docs.google.com/spreadsheets/d/1JB-RL9C8Xz6toUWTNhMpaWeLlYvva8qU/export?format=xlsx"

# Carregando os dados:
dados = pd.read_excel(url)

# Exibindo as 5 primeiras linhas:
print("Visualização inicial dos dados:")
print(dados.head())


```

Com essa visualização inicial já percebemos que existem colunas **numéricas**, **categóricas** e **textuais**, o que abre espaço para análises variadas:  
- Comparações entre gêneros e notas;  
- Relação entre votos e faturamento;  
- Possíveis insights extraídos da coluna *Overview*.  

Para entender melhor os dados, verifico os tipos de cada coluna e a presença de valores nulos. Isso permite identificar:

- Quais colunas são numéricas, categóricas ou textuais;
- Se há necessidade de tratar valores faltantes antes de prosseguir com a análise.

```python

# Verificando os tipos de colunas e valores nulos:
print("Tipos de colunas e quantidade de valores nulos em cada coluna:")
print(dados.info())

# Contando quantos valores nulos existem em cada coluna:
print("\nNúmero de valores nulos por coluna:")
print(dados.isnull().sum())

```
Observa-se que:

- Existem 16 colunas, das quais a maioria é do tipo textual (object), algumas são numéricas (int64 e float64);
- Algumas colunas apresentam valores nulos, sendo Certificate (101 nulos), Meta_score (157 nulos) e Gross (169 nulos);
- Colunas como IMDB_Rating, No_of_Votes e Meta_score são importantes para análises quantitativas, enquanto Genre, Director e Star1-4 permitem análises categóricas;

Para entender melhor os dados numéricos, verifico a distribuição básica de cada coluna numérica, como média, mediana, mínimo, máximo e quartis. Isso auxilia identificar padrões gerais e possíveis valores atípicos.

```python
# Estatísticas básicas das colunas numéricas:
estatisticas_numericas = dados.describe()
print(estatisticas_numericas)
```

Observo algumas informações importantes:

- A coluna Unnamed: 0 é apenas um índice e não carrega informação relevante para análise;
- As notas do IMDB_Rating variam entre 7.6 e 9.2, com média próxima de 7.95, indicando que a maior parte dos filmes tem avaliações altas;
- O Meta_score apresenta alguns valores faltantes (apenas 842 registros), e varia de 28 a 100, com média de aproximadamente 78;
- O número de votos (No_of_Votes) mostra grande variação, de cerca de 25 mil a mais de 2 milhões, indicando que alguns filmes são muito mais populares que outros.

Essas estatísticas iniciais ajudam a entender a distribuição geral e a identificar possíveis valores extremos, que podem influenciar análises futuras.

Para visualizar melhor a distribuição das colunas numéricas, gero histogramas simples de cada uma delas. Isso auxilia a identificar padrões, tendências e possíveis valores extremos.

```python
# Importando biblioteca de visualização:
import matplotlib.pyplot as plt

# Selecionando apenas as colunas numéricas relevantes:
colunas_numericas = ['IMDB_Rating', 'Meta_score', 'No_of_Votes']

# Criando histogramas para cada coluna numérica:
dados[colunas_numericas].hist(bins=10, figsize=(10,5))
plt.tight_layout()
plt.show()
```

<img width="989" height="490" alt="image" src="https://github.com/user-attachments/assets/47e2e4df-3109-4be8-a0db-2307f0dd3263" />

**Interpretação dos histogramas:**

•	**IMDB_Rating:** A distribuição de notas é estreita e concentrada entre aproximadamente 7.6 e 8.3, com poucos filmes acima de 8.5. Isso indica baixa variabilidade nas avaliações (desvio padrão pequeno), ou seja, a maioria dos títulos neste conjunto tem avaliações relativamente altas e próximas entre si. Para modelagem, isso significa que prever pequenas diferenças na nota pode ser mais difícil, pois a variável alvo tem pouca dispersão.

•	**Meta_score:** O meta score tem uma faixa maior (≈28 a 100) e parece concentrar-se em torno de valores entre 70 e 90. Há mais variabilidade em relação ao IMDB_Rating e alguns registros faltando. Como é uma medida de crítica especializada, tende a ser um bom preditor numérico da nota do IMDB, desde que tratemos os missing adequadamente.

•	**No_of_Votes:** Mostra forte assimetria à direita, “long tail”, muitos filmes com relativamente poucos votos e poucos filmes com dezenas/centenas de milhares. Essa assimetria justifica a transformação logarítmica antes de usar essa variável em análises ou modelos, para reduzir a influência dos outliers e facilitar a visualização.


Para avaliar a associação entre popularidade (número de votos) e qualidade percebida (IMDB_Rating), aplico uma transformação logarítmica em No_of_Votes e ploto a dispersão entre log_votes e IMDB_Rating. Faço também um plot entre Meta_score e IMDB_Rating. Por fim, calculo a correlação de Pearson entre IMDB_Rating e cada uma dessas variáveis para quantificar a força da associação.


```python
#Importação das bibliotecas:
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Criar coluna com log(1 + No_of_Votes):
dados['log_votes'] = np.log1p(dados['No_of_Votes'])

# Plots de dispersão lado a lado:
plt.figure(figsize=(12,4))

plt.subplot(1,2,1)
plt.scatter(dados['log_votes'], dados['IMDB_Rating'], s=10, alpha=0.6)
plt.xlabel('log(1 + No_of_Votes)')
plt.ylabel('IMDB_Rating')
plt.title('IMDB_Rating vs log(No_of_Votes)')

plt.subplot(1,2,2)
plt.scatter(dados['Meta_score'], dados['IMDB_Rating'], s=10, alpha=0.6)
plt.xlabel('Meta_score')
plt.ylabel('IMDB_Rating')
plt.title('IMDB_Rating vs Meta_score')

plt.tight_layout()
plt.show()

# Calcular correlações de Pearson (apenas pares com dados não-nulos):
corr_votes = dados[['IMDB_Rating', 'log_votes']].dropna().corr().loc['IMDB_Rating','log_votes']
corr_meta = dados[['IMDB_Rating', 'Meta_score']].dropna().corr().loc['IMDB_Rating','Meta_score']

print(f"Correlação Pearson IMDB_Rating x log_votes: {corr_votes:.3f}")
print(f"Correlação Pearson IMDB_Rating x Meta_score: {corr_meta:.3f}")

```
<img width="1189" height="390" alt="image" src="https://github.com/user-attachments/assets/4db2295c-97ef-4550-8a43-fccd83b4d0ca" />

**Análise dos gráficos e correlações:**

Os gráficos de dispersão mostram uma tendência positiva moderada:

- Filmes com maior número de votos (mais populares) tendem a ter avaliações mais altas no IMDB.

- Da mesma forma, quanto maior o Meta_score, maior tende a ser o IMDB_Rating.

Os coeficientes de correlação de Pearson confirmam essa leitura:

- **IMDB_Rating × log_votes = 0.318**, associação positiva moderada entre popularidade e avaliação do público.

- **IMDB_Rating × Meta_score = 0.271**, associação positiva, porém um pouco mais fraca, entre avaliação da crítica e avaliação do público.

Em resumo, tanto a popularidade quanto a crítica parecem influenciar as notas do IMDB, mas nenhuma das duas explica sozinha a variação nas avaliações.


Para entender o papel das variáveis categóricas, avalio a distribuição da quantidade de filmes por gênero e por classificação indicativa (Certificate). Isso ajuda a identificar quais categorias são mais frequentes no conjunto de dados e fornece uma visão inicial da composição do dataset.

```python
# Contagem de filmes por Gênero e por Classificação Indicativa:
import matplotlib.pyplot as plt

# Contagem de cada categoria:
contagem_genero = dados['Genre'].value_counts().head(10)   # 10 gêneros mais comuns
contagem_certificado = dados['Certificate'].value_counts()

# Plots:
plt.figure(figsize=(12,4))

plt.subplot(1,2,1)
contagem_genero.plot(kind='bar')
plt.title("Top 10 Gêneros mais frequentes")
plt.ylabel("Número de filmes")

plt.subplot(1,2,2)
contagem_certificado.plot(kind='bar')
plt.title("Distribuição por Classificação Indicativa")
plt.ylabel("Número de filmes")

plt.tight_layout()
plt.show()

# Mostrar tabelas:
print("Top 10 gêneros:")
print(contagem_genero)

print("\nDistribuição por classificação indicativa:")
print(contagem_certificado)

```

- **Gêneros (Genre):** O gênero Drama aparece como o mais frequente, com 84 filmes, seguido por combinações como Drama, Romance e Comedy, Drama. Isso mostra que a maior parte do dataset é composta por dramas puros ou misturados com romance, comédia e crime, indicando certa concentração temática.

- **Classificação indicativa (Certificate):** As classificações mais comuns são U (234 filmes), A (196 filmes), UA (175 filmes) e R (146 filmes), cobrindo a maior parte do acervo. As demais categorias aparecem com frequência bastante reduzida, o que pode dificultar análises mais robustas para elas.


Para investigar se determinados gêneros e classificações indicativas estão associados a melhores avaliações, calculo a média do IMDB_Rating por gênero e por classificação indicativa.

```python
import matplotlib.pyplot as plt

# Média do IMDB_Rating por gênero (10 mais frequentes):
media_genero = dados.groupby('Genre')['IMDB_Rating'].mean().sort_values(ascending=False).head(10)

# Média do IMDB_Rating por classificação indicativa:
media_certificado = dados.groupby('Certificate')['IMDB_Rating'].mean().sort_values(ascending=False)

# Plots:
plt.figure(figsize=(12,4))

plt.subplot(1,2,1)
media_genero.plot(kind='bar')
plt.title('Média IMDB_Rating por Gênero (Top 10)')
plt.ylabel('Média IMDB_Rating')

plt.subplot(1,2,2)
media_certificado.plot(kind='bar')
plt.title('Média IMDB_Rating por Classificação Indicativa')
plt.ylabel('Média IMDB_Rating')

plt.tight_layout()
plt.show()

```
<img width="1203" height="390" alt="image" src="https://github.com/user-attachments/assets/326d9f55-91ed-4cd5-9e09-22397685fe90" />

O gráfico mostra que as médias de IMDB_Rating variam pouco entre os diferentes gêneros e classificações indicativas. Alguns gêneros específicos, como “Animation, Drama, War” e “Action, Sci-Fi”, apresentam médias ligeiramente mais altas, mas de modo geral os valores ficam bastante próximos. O mesmo ocorre para as classificações: não há uma diferença clara de avaliação entre faixas etárias distintas. Isso sugere que a percepção de qualidade (IMDB_Rating) é relativamente estável independentemente da classificação indicativa ou do gênero.


Para identificar valores atípicos, utilizo boxplots das principais variáveis numéricas (IMDB_Rating, Meta_score e log_votes). Essa visualização ajuda a detectar filmes que estão muito acima ou abaixo da distribuição central.


```python
import matplotlib.pyplot as plt

# Selecionar variáveis numéricas:
variaveis = ['IMDB_Rating', 'Meta_score', 'log_votes']

# Criar boxplots:
plt.figure(figsize=(10,4))
dados[variaveis].boxplot()
plt.title("Boxplots das variáveis numéricas")
plt.ylabel("Valor")
plt.show()

```
<img width="850" height="374" alt="image" src="https://github.com/user-attachments/assets/c9eede1b-f5e5-46d8-9863-000b8a59711d" />


O boxplot demonstra pontos importantes:

- IMDB_Rating: distribuição bem concentrada entre 7.5 e 9. Poucos outliers para baixo.

- Meta_score: maior dispersão, com outliers bem visíveis abaixo de 40.

- log_votes: alguns filmes com valores bem acima da mediana, indicando títulos extremamente populares.


```python

```


```python

```
