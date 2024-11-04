# NYC Taxi Trip Data Pipeline

## Visão Geral

Este é um projeto de **Engenharia de Dados End-to-End**, desenvolvido com **PySpark** e **Databricks**, utilizando a **Arquitetura Medalhão** para organizar os dados em camadas (Bronze, Silver e Gold) e criando um **Data Lakehouse** local no **FileStore do Databricks** com arquivos em formato **Parquet**. O projeto se concentra na ingestão, transformação e análise dos dados de corridas de táxi de Nova York (NYC Taxi Trip Data), proporcionando uma visão completa e estruturada do processo de pipeline de dados.

## Arquitetura do Projeto

![image](https://github.com/user-attachments/assets/7d76e17e-8e92-4c4d-bc0f-3f3095a9905a)


1. **Camada Bronze**: Dados brutos ingeridos diretamente do dataset do NYC Taxi Trip.
2. **Camada Silver**: Dados transformados e limpos, prontos para análises mais detalhadas.
3. **Camada Gold**: Dados agregados e enriquecidos, otimizados para visualizações e insights finais.

## Estrutura do Pipeline

### 1. Ingestão de Dados - Camada Bronze

Nesta etapa, os dados são carregados no Databricks e armazenados em **formato Parquet** na camada Bronze, permitindo uma ingestão eficiente e uma estrutura de armazenamento otimizada para operações subsequentes.

### 2. Limpeza e Transformação de Dados - Camada Silver

A camada Silver se concentra em transformar e limpar os dados, garantindo que estejam prontos para análises mais avançadas. Aqui estão as principais modificações realizadas:

- **Remoção de duplicatas**: `df.dropDuplicates()` foi usado para garantir que cada entrada seja única.
- **Tratamento de valores nulos**: Decidiu-se substituir valores nulos em colunas específicas e remover registros com dados ausentes em colunas críticas.
- **Conversão de tipos de dados**:
  - Colunas de data e hora foram convertidas para tipo **Timestamp** para facilitar operações de data.
  - Colunas numéricas foram convertidas para tipos **Integer** ou **Float** onde aplicável.
- **Filtragem de viagens inválidas**:
  - Remoção de registros com distância zero ou negativa.
  - Filtragem de registros com tarifas negativas.

Os dados transformados foram armazenados em **formato Parquet** na camada Silver.

### 3. Agregação e Análise - Camada Gold

A camada Gold é destinada à agregação e análise dos dados limpos, visando gerar insights úteis para visualizações e relatórios. Aqui estão as principais agregações realizadas:

- **Cálculo da duração da viagem**: Calculado com base na diferença entre os horários de início e fim da corrida.
- **Tempo médio de viagem por região e horário**:
  - Agrupamento dos dados por região de partida e hora do dia, calculando o tempo médio de viagem com `avg()`.
- **Análise de gorjetas por período**:
  - Divisão dos dados por períodos (ex.: dia, semana, mês), com cálculos de média e soma de gorjetas usando `groupBy()` e `avg()`/`sum()`.
- **Identificação das rotas mais populares**:
  - Agrupamento por combinações de zonas de partida e chegada, determinando as rotas mais frequentes com `count()` e ordenação em ordem decrescente.
- **Distribuição de tipo de pagamento**:
  - Contagem do número de corridas por tipo de pagamento, facilitando a análise da preferência de pagamento entre os clientes.

Os resultados das agregações foram armazenados em **formato Parquet** na camada Gold, prontos para serem utilizados em visualizações e dashboards.

## Estrutura de Pastas

A estrutura de pastas reflete a organização das camadas do pipeline, como ilustrado abaixo:

```
NYC_Taxi_Project/
├── notebooks/
│   ├── 01_Ingestao_Bronze.ipynb
│   ├── 02_Transformacao_Silver.ipynb
│   └── 03_Agregacao_Gold.ipynb
├── FileStore/
│   ├── bronze/
│   ├── silver/
│   └── gold/
```

## Visualização

Para a visualização dos dados, utilizamos o **Looker Studio** para criar dashboards que ilustram métricas chave, como:

- **Total de viagens** e **duração média de viagem**
- **Distância média de viagem**
- **Valor médio das corridas**
- **Distribuição de métodos de pagamento**
- **Principais zonas de partida**
  
  ![image](https://github.com/user-attachments/assets/ee5eeb16-cc00-4e17-9894-1d4c115119d5)

## Conclusão

Este projeto fornece uma visão detalhada e prática da aplicação da **Engenharia de Dados** usando PySpark e Databricks, com uma estrutura organizada em camadas que facilita a gestão, transformação e análise dos dados de forma escalável e eficiente. Através da Arquitetura Medalhão e do uso do Lakehouse no Databricks, o projeto demonstra um pipeline completo, desde a ingestão de dados brutos até a geração de insights visuais.

---
