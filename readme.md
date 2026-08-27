# 📊 Python Insights — Análise de Cancelamento de Clientes

## 📌 Sobre o projeto

Este projeto foi desenvolvido com o objetivo de analisar uma base de dados de clientes e identificar os principais fatores relacionados ao **cancelamento de serviços (churn)**.

A análise simula um cenário real de uma empresa com uma grande quantidade de clientes, na qual uma parcela significativa da base está inativa. A partir dos dados disponíveis, o objetivo é entender **por que os clientes estão cancelando** e identificar possíveis ações que poderiam contribuir para a redução da taxa de cancelamento.

O projeto foi desenvolvido acompanhando uma aula prática da **Hashtag Programação**, com foco no aprendizado e aplicação de conceitos de análise de dados utilizando Python.

---

## 🎯 Objetivos

Durante o projeto, foram trabalhados os seguintes objetivos:

* Importar e visualizar uma base de dados;
* Identificar possíveis problemas nos dados;
* Realizar o tratamento e limpeza da base;
* Analisar a quantidade e percentual de clientes que cancelaram;
* Identificar fatores relacionados ao cancelamento;
* Criar visualizações para facilitar a interpretação dos dados;
* Encontrar padrões de comportamento dos clientes;
* Propor possíveis ações para reduzir o número de cancelamentos.

---

## 🛠️ Tecnologias e ferramentas

* **Python**
* **Pandas** — manipulação e tratamento dos dados
* **Plotly Express** — criação de gráficos interativos
* **Jupyter Notebook**
* **CSV** — formato da base de dados

---

## 📂 Estrutura da análise

### 1. Importação dos dados

A base de clientes foi importada utilizando a biblioteca Pandas:

```python
import pandas as pd

tabela = pd.read_csv('cancelamentos.csv')
```

---

### 2. Visualização inicial

Primeiramente, foi realizada uma análise da estrutura da base para compreender quais informações estavam disponíveis.

Também foi removida a coluna `CustomerID`, por não ser relevante para a análise:

```python
tabela = tabela.drop(columns='CustomerID')

display(tabela)
```

---

### 3. Tratamento dos dados

Foi realizada uma verificação inicial das informações disponíveis na base.

Também foram identificados valores ausentes. Como a quantidade de registros incompletos era pequena, optou-se pela remoção dessas linhas:

```python
tabela = tabela.dropna()
```

Essa etapa foi importante para garantir maior consistência durante as análises posteriores.

---

### 4. Análise inicial do cancelamento

A coluna `cancelou` foi utilizada como principal variável de análise.

Primeiramente, foi realizada a contagem de clientes que permaneceram ativos e daqueles que cancelaram:

```python
tabela['cancelou'].value_counts()
```

Em seguida, foi calculado o percentual de cada grupo:

```python
tabela['cancelou'].value_counts(normalize=True)
```

Essa análise permitiu compreender o tamanho do problema de churn dentro da base.

---

## 📈 5. Análise dos fatores relacionados ao cancelamento

Para entender quais características poderiam estar relacionadas ao cancelamento, foram criados gráficos para as diferentes colunas da base.

```python
import plotly.express as px

for coluna in tabela.columns:
    grafico = px.histogram(
        tabela,
        x=coluna,
        color='cancelou',
        text_auto=True
    )
    
    grafico.show()
```

Através dessas visualizações, foi possível identificar alguns padrões importantes.

### 🔎 Principais insights

#### 📅 Duração do contrato

Clientes com contrato **mensal (Monthly)** apresentaram uma taxa de cancelamento muito elevada.

**Possível ação:**

* Incentivar contratos de maior duração;
* Oferecer descontos ou benefícios para planos trimestrais e semestrais;
* Criar campanhas de migração do plano mensal para contratos mais longos.

---

#### ☎️ Número de ligações para o Call Center

Foi observado um aumento significativo dos cancelamentos entre clientes que realizaram muitas ligações para o atendimento.

Clientes com **mais de 4 ligações** apresentaram um comportamento de alto risco de cancelamento.

**Possível ação:**

Criar um sistema de alerta para identificar clientes que entram repetidamente em contato com o suporte e realizar uma abordagem preventiva para solucionar o problema antes que ocorra o cancelamento.

---

#### 💳 Dias de atraso no pagamento

Também foi identificado um relacionamento entre atrasos no pagamento e cancelamento.

Clientes com atrasos superiores a **20 dias** apresentaram maior tendência ao cancelamento.

**Possível ação:**

Criar um alerta preventivo para clientes com aproximadamente 15 dias de atraso, permitindo uma abordagem antes que o cliente atinja uma situação de maior risco.

---

## 🚨 6. Aplicação das estratégias

Após identificar alguns dos principais fatores relacionados ao cancelamento, foi realizado um filtro na base considerando condições consideradas mais favoráveis:

### Contrato

Remoção dos clientes com contrato mensal:

```python
condicao = tabela['duracao_contrato'] != 'Monthly'
tabela = tabela[condicao]
```

### Ligações para o Call Center

Consideração de clientes com até 4 ligações:

```python
condicao = tabela['ligacoes_callcenter'] <= 4
tabela = tabela[condicao]
```

### Dias de atraso

Consideração de clientes com até 20 dias de atraso:

```python
condicao = tabela['dias_atraso'] <= 20
tabela = tabela[condicao]
```

Após a aplicação dessas condições, foi realizada uma nova análise da taxa de cancelamento:

```python
display(tabela['cancelou'].value_counts(normalize=True))
```

O objetivo dessa etapa foi demonstrar como a aplicação de estratégias baseadas nos dados pode impactar o indicador de cancelamento.

---

## 💡 Principais aprendizados

Este projeto permitiu praticar conceitos importantes de **Análise de Dados com Python**, incluindo:

* Importação de bases de dados;
* Exploração de dados;
* Tratamento de valores ausentes;
* Manipulação de DataFrames;
* Filtros e condições;
* Análise de variáveis categóricas;
* Cálculo de proporções;
* Visualização de dados;
* Identificação de padrões;
* Transformação de dados em insights para tomada de decisão.

Além da parte técnica, o projeto demonstra uma etapa fundamental da área de Dados: **não basta analisar os números; é necessário transformar os resultados em informações que possam apoiar decisões de negócio.**

---

## 📊 Resultado

A análise mostrou que determinados comportamentos dos clientes podem estar associados a uma maior probabilidade de cancelamento.

Entre os principais pontos identificados estão:

| Fator                          | Possível risco | Estratégia                              |
| ------------------------------ | -------------- | --------------------------------------- |
| Contrato mensal                | 🔴 Alto        | Incentivar contratos mais longos        |
| Muitas ligações ao Call Center | 🔴 Alto        | Criar alerta e atendimento preventivo   |
| Atrasos no pagamento           | 🟠 Atenção     | Contato antes de atingir atraso crítico |

A partir desses insights, a empresa poderia desenvolver estratégias de **retenção de clientes**, utilizando os próprios dados para identificar clientes em situação de risco e agir preventivamente.

---

## 🚀 Próximos passos

Como evolução do projeto, algumas possibilidades seriam:

* Criar um **dashboard interativo** com os principais indicadores;
* Utilizar **SQL** para realizar consultas na base;
* Criar uma análise mais aprofundada de cada variável;
* Desenvolver um modelo de **Machine Learning para previsão de churn**;
* Criar um sistema de classificação de clientes por nível de risco;
* Automatizar a atualização dos indicadores;
* Criar recomendações personalizadas para retenção de clientes.

---

## 📚 Fonte de estudo

Projeto desenvolvido como prática acompanhando uma aula da **Hashtag Programação**, com adaptações e aplicação dos conceitos estudados.

O objetivo principal foi utilizar o projeto como oportunidade de aprendizado e prática em **Python, Pandas, análise exploratória e visualização de dados**.

---

## 👩‍💻 Autora

**Jessy Godoy**

Estudante de Desenvolvimento de Sistemas | Python | Dados | Back-End | Cybersecurity

Este projeto faz parte da minha jornada de transição para a área de Tecnologia e representa uma aplicação prática dos conhecimentos adquiridos durante meus estudos em Python e Análise de Dados.
