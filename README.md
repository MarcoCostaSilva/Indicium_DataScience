# 🎬 Desafio Lighthouse – Análise Cinematográfica e Previsão de Nota IMDB

## 📝 Introdução

Este projeto faz parte do **Desafio Lighthouse da Indicium**, focado na área de **Data Science**.  
O objetivo é analisar dados de filmes e fornecer insights para auxiliar na decisão de **qual tipo de filme deve ser produzido a seguir**, além de desenvolver um modelo preditivo para estimar a nota do IMDB. A seguir, estão descritas todas as etapas do projeto, com explicações detalhadas sobre cada fase da análise e modelagem.

---

## 🎯 Objetivos do Projeto

- 🔹 Explorar e entender a base de dados cinematográficos;  
- 🔹 Gerar insights sobre gêneros, faturamento, notas e sinopses;  
- 🔹 Responder perguntas específicas de negócios e análise de dados;  
- 🔹 Desenvolver um modelo preditivo para IMDB_Rating;  
- 🔹 Documentar o processo completo de forma clara e organizada;  

---

## 🗂 Estrutura do Desafio

### 1️⃣ Análise Exploratória de Dados (EDA)
- Exploração inicial das variáveis; 
- Criação de gráficos e tabelas resumidas;  
- Identificação de padrões e hipóteses iniciais;  

### 2️⃣ Respostas a perguntas estratégicas
- Qual filme recomendar para uma pessoa que você não conhece?  
- Quais fatores influenciam o faturamento esperado de um filme?  
- O que a coluna *Overview* revela sobre o gênero ou características do filme?  

### 3️⃣ Modelagem Preditiva
- Variáveis e transformações utilizadas; 
- Tipo de problema: **Regressão**;  
- Escolha do modelo e justificativa;  
- Métricas de avaliação; 

### 4️⃣ Previsão para um filme específico
- Filme: *The Shawshank Redemption*;  
- Previsão da nota IMDB com o modelo treinado; 

### 5️⃣ Entregáveis finais
- Modelo salvo em `.pkl`;  
- Notebook ou script Python com o passo a passo;  
- Relatórios de análise;  
- Arquivo `requirements.txt` com pacotes utilizados;
- README detalhado com instruções e documentação;  

---

## 📊 Dicionário de Dados

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

Nesta primeira etapa, carego a base de dados e realizo uma **análise exploratória inicial (EDA)**.  
O objetivo é conhecer melhor a estrutura da tabela, verificar o tamanho do conjunto de dados, observar os tipos de variáveis e identificar se existem valores ausentes.  
Essa etapa é importante porque nos ajuda a entender quais informações estão disponíveis e como elas poderão ser utilizadas nas análises e modelagens seguintes.

## Carregamento dos Dados

O código abaixo realiza a leitura da planilha no Google Colab, identifica as abas e exibe as primeiras linhas do dataset:

```python
# Importando biblioteca
import pandas as pd

caminho_arquivo = "/content/desafio_indicium_imdb.xlsx"

# Descobrindo quais abas existem na planilha
abas = pd.ExcelFile(caminho_arquivo)
print("Abas encontradas no arquivo:", abas.sheet_names)

dados = pd.read_excel(caminho_arquivo, sheet_name=abas.sheet_names[0])

# Exibindo as 5 primeiras linhas para confirmar que carregou
print("Visualização inicial dos dados:")
print(dados.head())

```python

Com essa visualização inicial já percebemos que existem colunas **numéricas**, **categóricas** e **textuais**, o que abre espaço para análises variadas:  
- Comparações entre gêneros e notas;  
- Relação entre votos e faturamento;  
- Possíveis insights extraídos da coluna *Overview*.  

Essa primeira inspeção é fundamental para guiar os próximos passos da **Análise Exploratória de Dados (EDA)**.
