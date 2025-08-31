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
import pandas as pd

caminho_arquivo = "/content/desafio_indicium_imdb.xlsx"

# Carregando os dados diretamente
dados = pd.read_excel(caminho_arquivo)

# Exibindo as 5 primeiras linhas para confirmar que carregou
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




```python

```






