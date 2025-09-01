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

## 1️⃣ – Análise Exploratória dos Dados (EDA)

O objetivo é conhecer melhor a estrutura da tabela, verificar o tamanho do conjunto de dados, observar os tipos de variáveis e identificar se existem valores ausentes. Essa etapa é importante porque nos ajuda a entender quais informações estão disponíveis e como elas poderão ser utilizadas nas análises e modelagens seguintes.

## Carregamento dos Dados

```python
# Importando biblioteca:
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


Para aprofundar a análise dos boxplots, identifico os filmes que se destacam como outliers nas distribuições de IMDB_Rating, Meta_score e log_votes. Essa etapa permite contextualizar os pontos extremos e verificar se eles representam casos especiais ou dados problemáticos.

```python
# Função para detectar outliers usando regra do IQR
def detectar_outliers(coluna):
    Q1 = dados[coluna].quantile(0.25)
    Q3 = dados[coluna].quantile(0.75)
    IQR = Q3 - Q1
    limite_inferior = Q1 - 1.5 * IQR
    limite_superior = Q3 + 1.5 * IQR
    return dados[(dados[coluna] < limite_inferior) | (dados[coluna] > limite_superior)]

# Detectar outliers em cada variável
outliers_rating = detectar_outliers('IMDB_Rating')
outliers_meta = detectar_outliers('Meta_score')
outliers_votes = detectar_outliers('log_votes')

print("Outliers IMDB_Rating:")
print(outliers_rating[['Series_Title','IMDB_Rating']].head())

print("\nOutliers Meta_score:")
print(outliers_meta[['Series_Title','Meta_score']].head())

print("\nOutliers log_votes:")
print(outliers_votes[['Series_Title','No_of_Votes','log_votes']].head())

```
- **IMDB_Rating:** os outliers são filmes consagrados com notas extremamente altas, como The Godfather e The Dark Knight. Isso mostra que, nesse caso, os outliers não são erros, mas títulos muito famosos que se destacam no ranking.

- **Meta_score:** os outliers são filmes com notas muito baixas da crítica especializada, como Tropa de Elite (33) e I Am Sam (28). Esses casos são interessantes para investigar discrepâncias entre crítica e público.
  
- **log_votes:** não foram detectados outliers, o que sugere que a popularidade dos filmes (medida por votos) é mais homogênea após a transformação logarítmica.


## **Principais achados:**
```python
print("Resumo da Análise Exploratória de Dados (EDA):\n")

print("1. Distribuição das variáveis numéricas:")
print(dados[['IMDB_Rating','Meta_score','log_votes']].describe(), "\n")

print("2. Correlação entre variáveis numéricas:")
print(dados[['IMDB_Rating','Meta_score','log_votes']].corr(), "\n")

print("3. Gêneros mais frequentes:")
print(dados['Genre'].value_counts().head(5), "\n")

print("4. Classificação indicativa mais frequente:")
print(dados['Certificate'].value_counts().head(5), "\n")

print("5. Exemplos de outliers:")
print("Notas muito altas:", outliers_rating['Series_Title'].tolist()[:3])
print("Notas da crítica muito baixas:", outliers_meta['Series_Title'].tolist()[:3])

```
## **Conclusões da Etapa 01:** 

- A Análise Exploratória de Dados (EDA) permitiu identificar padrões e características importantes do conjunto de filmes analisado. As variáveis numéricas (IMDB_Rating, Meta_score e log_votes) apresentaram distribuições concentradas, com média de avaliação do público em torno de 7,9 pontos, enquanto as notas da crítica (Meta_score) variaram mais, com presença de valores extremos baixos. A transformação logarítmica em log_votes reduziu a assimetria, resultando em uma distribuição mais homogênea de popularidade.

- As correlações mostraram associação positiva moderada entre IMDB_Rating e log_votes (0,318), indicando que filmes mais bem avaliados tendem também a receber mais votos. A relação entre IMDB_Rating e Meta_score foi mais fraca (0,271), sugerindo diferenças entre a percepção do público e da crítica.

- Na análise categórica, o gênero Drama foi o mais recorrente, tanto de forma isolada quanto combinado com Romance e Comédia. Em termos de classificação indicativa, a maioria dos títulos se concentrou em certificados U, A e UA, voltados ao público geral, seguidos por filmes classificados como R.

- A investigação de outliers revelou títulos de destaque nas avaliações do público, como The Godfather e The Dark Knight, que representam casos de sucesso reconhecidos mundialmente. Por outro lado, alguns filmes apresentaram notas muito baixas pela crítica, como Tropa de Elite e I Am Sam, evidenciando divergências relevantes entre público e avaliadores especializados.

- Em resumo, a EDA trouxe uma visão inicial clara sobre a distribuição das notas, a popularidade, os gêneros predominantes, as classificações etárias e os filmes de comportamento extremo, fornecendo subsídios para análises mais aprofundadas em etapas futuras.


## 2️⃣ **Respostas a perguntas estratégicas:**

### **2.1 - Qual filme recomendar para uma pessoa que você não conhece?**

Se não conhecemos o perfil da pessoa, a melhor recomendação é baseada nos filmes com maior avaliação do público (IMDB_Rating) e ao mesmo tempo alta popularidade (log_votes). Isso garante que a indicação seja de um título amplamente reconhecido.

```python
import numpy as np

# Garante a coluna de popularidade em escala log:
if 'log_votes' not in dados.columns:
    dados['log_votes'] = np.log1p(dados['No_of_Votes'])

# Ranking de recomendação geral (qualidade + popularidade):
top_filmes = (
    dados[['Series_Title', 'IMDB_Rating', 'No_of_Votes', 'log_votes']]
    .dropna(subset=['IMDB_Rating', 'No_of_Votes'])
    .sort_values(by=['IMDB_Rating', 'log_votes'], ascending=[False, False])
    .head(10)
)

print(top_filmes.to_string(index=False))
```
Os filmes mais indicados para recomendação geral são clássicos reconhecidos tanto pela crítica quanto pelo público, com notas altas (IMDB_Rating ≥ 8.8) e enorme volume de votos. títulos como The Godfather, The Dark Knight, Pulp Fiction e Inception aparecem no topo por unirem qualidade percebida e popularidade, tornando-os escolhas seguras quando não se conhece o perfil da pessoa.

### **2.2 - Quais fatores influenciam o faturamento esperado de um filme?**
Para investigar os fatores associados ao faturamento esperado (Gross), analiso as correlações entre esta variável e outros indicadores numéricos (imdb_rating, meta_score, log_votes). isso ajuda a identificar quais características parecem estar mais relacionadas ao desempenho financeiro de um filme.

```python
# Converter a coluna Gross para numérica (remover vírgulas e transformar em float):
dados['Gross'] = dados['Gross'].astype(str).str.replace(',', '', regex=False)
dados['Gross'] = pd.to_numeric(dados['Gross'], errors='coerce')

# Selecionar colunas relevantes e remover nulos:
dados_faturamento = dados[['Gross', 'IMDB_Rating', 'Meta_score', 'log_votes']].dropna()

# Calcular correlações de Pearson:
correlacoes = dados_faturamento.corr(method='pearson')

print("Correlação entre faturamento (Gross) e variáveis explicativas:")
print(correlacoes['Gross'].sort_values(ascending=False))
```
A variável mais associada ao faturamento (Gross) é o número de votos em escala logarítmica (correlação de 0.54), indicando que a popularidade do filme é um bom preditor de receita. a nota do público (IMDB_Rating) tem correlação fraca e positiva (0.13), sugerindo influência limitada. já a avaliação da crítica (Meta_score) praticamente não se correlaciona com faturamento (-0.03), indicando que não afeta diretamente a bilheteria.

### **2.3 - O que a coluna *Overview* revela sobre o gênero ou características do filme?**

Para investigar a coluna Overview, vamos gerar uma nuvem de palavras (WordCloud) com os termos mais frequentes. Isso nos permitirá identificar temas e padrões recorrentes nas descrições dos filmes, o que pode dar pistas sobre gêneros e características narrativas.

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt

# Juntar todos os textos da coluna Overview em uma única string:
texto_overview = " ".join(dados["Overview"].dropna().astype(str))

# Gerar a nuvem de palavras:
wordcloud = WordCloud(width=800, height=400, background_color="white").generate(texto_overview)

# Mostrar a nuvem:
plt.figure(figsize=(12,6))
plt.imshow(wordcloud, interpolation="bilinear")
plt.axis("off")
plt.title("Palavras mais frequentes nas sinopses (Overview)")
plt.show()
```
<img width="944" height="504" alt="image" src="https://github.com/user-attachments/assets/876cecf9-61d5-4c64-805a-3285a07a2225" />

A nuvem de palavras mostra que termos como life, man, young, family, love, world, two, story aparecem com muita frequência. Isso sugere que as sinopses destacam temas universais relacionados a relações pessoais, juventude, amor, conflitos e vida em sociedade, características comuns a dramas, romances e filmes familiares.


```python

```

```python

```
