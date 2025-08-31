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
