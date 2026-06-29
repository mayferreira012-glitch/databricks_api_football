# Campeonato Brasileiro Série A 2026 - Data Engineering Pipeline

Projeto de Engenharia de Dados desenvolvido na plataforma Databricks para ingestão, processamento, modelagem e visualização das estatísticas do Campeonato Brasileiro Série A 2026.
Os dados são obtidos através da Football API e processados seguindo a arquitetura **Medallion (Bronze, Silver e Gold)**, permitindo a construção de um pipeline analítico escalável para acompanhamento do campeonato.
Todo o projeto foi desenvolvido no **Databricks**, utilizando **PySpark**, **Delta Lake**, **Databricks Workflows**, **Databricks SQL Dashboards** e versionado através da integração nativa entre **Databricks** e **GitHub**.

---

# Arquitetura

```text
                       Football API
                             │
                             ▼
                     Bronze (Raw Layer)
                             │
        ┌────────────────────│───────────────┐──────────────┐
        ▼                    ▼               ▼              ▼  
    Silver                Silver           Silver          Silver  
   Matches                 Goals            Cards           Statistics
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼
       Gold Team        Gold Players      Gold Accuracy
        Statistics        Statistics        Statistics
                             │
                             ▼
                  Databricks SQL Dashboard
```

---

# Objetivos

- Automatizar a coleta dos dados do Campeonato Brasileiro Série A.
- Construir um pipeline completo de Engenharia de Dados utilizando arquitetura Lakehouse.
- Disponibilizar dados confiáveis para análises esportivas.
- Consolidar indicadores por equipes e jogadores.
- Criar dashboards interativos utilizando Databricks SQL.

---

# Stack Tecnológica

- Databricks
- Apache Spark (PySpark)
- Delta Lake
- Databricks Workflows
- Databricks SQL
- REST API
- Python
- SQL
- GitHub

---

# Pipeline

O pipeline segue a arquitetura Medallion.

## Bronze

Responsável pela ingestão dos dados brutos provenientes da Football API.
Características:
- armazenamento do payload original
- rastreabilidade
- possibilidade de reprocessamento
- persistência em Delta Lake
Tabela:
- bronze.matches_raw

---

## Silver

Nesta camada ocorre o tratamento, limpeza, padronização e modelagem dos dados.

### matches

Granularidade:
> 1 registro por partida
Contém:
- informações da partida
- equipes
- rodada
- estádio
- árbitro
- placar
- status

---

### goals

Granularidade:
> 1 registro por gol
Contém:
- jogador
- equipe
- minuto
- placar após o gol

---

### cards

Granularidade:
> 1 registro por cartão
Contém:
- jogador
- equipe
- tipo do cartão
- minuto

---

### statistics

Tabela consolidada contendo as estatísticas das equipes em cada partida.
Durante o processamento foram utilizadas técnicas como:
- explode de estruturas JSON
- pivot de dados
- tratamento de duplicidades
- normalização de colunas
- consolidação de estatísticas do mandante e visitante
Entre as métricas disponíveis:
- posse de bola
- finalizações
- finalizações no gol
- finalizações bloqueadas
- finalizações para fora
- escanteios
- impedimentos
- faltas
- cartões
- passes
- precisão dos passes
- defesas do goleiro
- substituições
- pênaltis

---

# Gold

A camada Gold disponibiliza datasets prontos para consumo analítico.

## team_statistics

Granularidade:
> 1 registro por equipe
Indicadores:
- classificação
- partidas
- vitórias
- empates
- derrotas
- pontos
- gols marcados
- gols sofridos
- saldo de gols
- cartões amarelos
- cartões vermelhos
- escanteios
- faltas
- impedimentos
- substituições
- aproveitamento (%)

---

## players_statistics

Granularidade:
> 1 registro por jogador
Indicadores:
- gols
- assistências
- cartões amarelos
- cartões vermelhos
Também são calculados rankings utilizando Window Functions para:
- artilharia
- assistências
- disciplina

---

## accuracy_statistics

Tabela responsável pelo cálculo dos indicadores de eficiência das equipes.
Métricas:
- percentual de passes certos
- percentual de finalizações no gol
- percentual de finalizações bloqueadas
- percentual de finalizações para fora

---

# Orquestração

Toda a execução do pipeline é realizada através de um **Databricks Workflow**, definido em arquivo YAML.

Fluxo:

```text
                         Football API
                              │
                              ▼
                    ┌─────────────────┐
                    │   bronze_api    │
                    └─────────────────┘
                              │
      ┌───────────────┬────────┼──────────────┬────────────────┐
      ▼               ▼        ▼              ▼
┌────────────┐ ┌──────────┐ ┌──────────┐ ┌─────────────────┐
│  matches   │ │  goals   │ │  cards   │ │   statistics    │
└────────────┘ └──────────┘ └──────────┘ └─────────────────┘
      │             │             │              │
      │             └──────┐      │              │
      │                    ▼      ▼              │
      │             ┌──────────────────┐         │
      │             │ gold_players     │         │
      │             └──────────────────┘         │
      │                                          │
      ├──────────────────────┐                   │
      │                      ▼                   ▼
      │             ┌──────────────────┐  ┌──────────────────┐
      └────────────►│    gold_team     │  │  gold_accuracy   │
                    └──────────────────┘  └──────────────────┘
                              │                  │
                              └────────┬─────────┘
                                       ▼
                             ┌──────────────────┐
                             │    Dashboard     │
                             └──────────────────┘


```

Características:
- execução automática
- controle de dependências
- política de retry
- versionamento da infraestrutura
- processamento em camadas

---

# Dashboard

O projeto disponibiliza um Dashboard desenvolvido no Databricks SQL contendo quatro páginas principais.

## Classificação

- tabela completa do campeonato
- pontos
- saldo de gols
- aproveitamento
- cartões
- filtro por equipe

---

## Jogadores

- ranking de artilheiros
- ranking de assistências
- ranking de cartões amarelos
- ranking de cartões vermelhos

---

## Estatísticas Gerais

Comparação entre todas as equipes.
Visualizações:
- gols marcados
- precisão de passes
- eficiência nas finalizações
- cartões recebidos

---

## Estatísticas por Equipe

Painel individual contendo:
- escanteios
- pênaltis
- impedimentos
- substituições

---

# Estrutura do Projeto

```text
project
│
├── bronze/
│   └── bronze.ipynb
│
├── silver/
│   ├── matches.ipynb
│   ├── goals.ipynb
│   ├── cards.ipynb
│   └── statistics.ipynb
│
├── gold/
│   ├── team_statistics.ipynb
│   ├── players_statistics.ipynb
│   └── accuracy_statistics.ipynb
│
├── orchestration/
│   └── API_Campeonato_Brasileiro.yaml
│
├── dashboards/
│   └── Campeonato_Brasileiro_Serie_A_2026.lvdash.json
│
└── README.md
```

---

# Conceitos de Engenharia de Dados Aplicados

- Medallion Architecture
- Lakehouse Architecture
- Delta Lake
- Apache Spark
- PySpark
- ETL
- Data Modeling
- Data Quality
- Incremental Processing
- Window Functions
- Pivot
- Explode de JSON
- Delta Tables
- Databricks Workflows
- Databricks SQL Dashboards
- Versionamento Git/GitHub
- Orquestração de Pipelines

---

# Próximas Evoluções

Possíveis melhorias futuras:
- Implementação de testes automatizados de qualidade dos dados.
- Monitoramento do pipeline.
- Integração com CI/CD utilizando Databricks Asset Bundles.
- Deploy automatizado entre ambientes.
- Ingestão em streaming para atualização em tempo real.
