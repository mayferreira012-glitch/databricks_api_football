# Campeonato Brasileiro Série A 2026 - Data Platform

---

## Status do Projeto

🚧 Em desenvolvimento: Atualmente o projeto possui a ingestão da camada Bronze e três tabelas Silver (matches, goals e cards).

---

## Visão Geral

Este projeto tem como objetivo construir uma plataforma analítica para acompanhamento e exploração das estatísticas do Campeonato Brasileiro Série A 2026, utilizando arquitetura Lakehouse no Databricks.
Os dados são obtidos diariamente por meio de uma API de futebol e armazenados seguindo a arquitetura Medallion (Bronze, Silver e Gold), permitindo a evolução gradual dos dados desde a ingestão bruta até a disponibilização para análises e dashboards.
Todo o desenvolvimento está sendo realizado no Databricks e versionado através da integração nativa entre Databricks e GitHub.

---

## Objetivos

* Automatizar a coleta de dados do Campeonato Brasileiro Série A 2026.
* Construir uma camada de dados confiável e escalável.
* Disponibilizar métricas e indicadores para análise de desempenho de clubes e jogadores.
* Aplicar boas práticas de Engenharia de Dados utilizando Delta Lake e Databricks.
* Criar dashboards analíticos para acompanhamento da competição.

---

## Arquitetura

```text
API Football
     │
     ▼
 Bronze Layer
     │
     ▼
 Silver Layer
     │
     ▼
 Gold Layer
     │
     ▼
 Dashboards
```

### Tecnologias Utilizadas

* Databricks
* Apache Spark (PySpark)
* Delta Lake
* GitHub
* REST API
* SQL
* Python

---

## Camada Bronze

A camada Bronze é responsável pela ingestão dos dados brutos retornados pela API.
Características:
* Armazena o payload original da API.
* Mantém a estrutura mais próxima possível da origem.
* Permite rastreabilidade e reprocessamento.
* Controle de carga utilizando recursos de Delta Lake.

Tabela atual:
### bronze.matches_raw

Contém o retorno completo da API para cada partida do campeonato, incluindo:
* Informações da partida
* Times
* Escalações
* Estatísticas
* Gols
* Cartões
* Substituições
* Eventos diversos

---

## Camada Silver

A camada Silver é responsável pela padronização, limpeza e modelagem dos dados para consumo analítico.
Atualmente o projeto possui as seguintes tabelas:

### silver.matches

Tabela fato contendo uma linha por partida.
Principais informações:
* Identificador da partida
* Data da partida
* Rodada
* Times mandante e visitante
* Placar final
* Placar do intervalo
* Estádio
* Árbitro
* Status da partida
Objetivo:
Disponibilizar uma visão consolidada de cada jogo realizado.

### silver.goals
Tabela contendo uma linha para cada gol registrado.
Principais informações:
* Match ID
* Time responsável pelo gol
* Jogador
* Minuto do gol
* Placar após o gol
Objetivo:
Permitir análises detalhadas de desempenho ofensivo, artilharia e evolução dos placares.

### silver.cards
Tabela contendo uma linha para cada cartão aplicado durante as partidas.
Principais informações:
* Match ID
* Jogador
* Equipe
* Tipo de cartão (amarelo ou vermelho)
* Minuto da ocorrência
Objetivo:
Permitir análises disciplinares de equipes e jogadores.

---

## Estrutura Atual do Projeto

```text
project
│
├── bronze/
│   └── bronze_matches_raw
│
├── silver/
│   ├── matches
│   ├── goals
│   └── cards
│
├── notebooks/
│   ├── bronze.ipynb
│   ├── matches.ipynb
│   ├── goals.ipynb
│   └── cards.ipynb
│
└── README.md
```

---

## Próximos Passos

### Expansão da camada Silver
Criação de nova tabela para exploração de estatísticas como:
* Faltas
* Penâltis
* Escanteios

### Criação da camada Gold
Construção de modelos analíticos.

### Dashboards
Desenvolvimento de dashboards no Databricks para visualização.

---

## Boas Práticas Aplicadas

* Arquitetura Medallion
* Delta Lake
* Processamento incremental
* Padronização de nomenclaturas
* Separação entre camadas de ingestão e consumo
* Versionamento de notebooks via GitHub
* Reprodutibilidade dos pipelines


