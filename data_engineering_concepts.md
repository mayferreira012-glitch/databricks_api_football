## Data Engineering Concepts

Este projeto foi desenvolvido seguindo conceitos e padrões amplamente utilizados em plataformas modernas de Engenharia de Dados.

### Medallion Architecture
A organização dos dados segue o padrão Medallion Architecture:

* **Bronze:** armazenamento dos dados brutos recebidos da API.
* **Silver:** tratamento, limpeza, normalização e modelagem dos dados.
* **Gold:** camada analítica com agregações e métricas de negócio para consumo por dashboards e análises.

Essa abordagem aumenta a confiabilidade dos dados e simplifica processos de manutenção e evolução do pipeline.

---

### Delta Lake
Todas as tabelas são armazenadas utilizando Delta Lake, permitindo:
* Transações ACID
* Controle de versões dos dados
* Melhor desempenho de leitura e escrita
* Evolução de schema
* Processos de reprocessamento mais seguros

---

### Incremental Data Processing
O pipeline foi projetado para suportar cargas incrementais, reduzindo processamento desnecessário e melhorando a eficiência operacional.
Benefícios:
* Menor tempo de execução
* Menor consumo de recursos computacionais
* Facilidade de atualização diária dos dados

---

### Data Modeling
A camada Silver adota um modelo orientado a eventos, onde cada tabela representa uma entidade específica do domínio:

| Tabela  | Granularidade          |
| ------- | ---------------------- |
| matches | 1 registro por partida |
| goals   | 1 registro por gol     |
| cards   | 1 registro por cartão  |

Essa estratégia facilita análises detalhadas e futuras agregações na camada Gold.

---

### Data Quality
Durante a transformação dos dados são aplicadas validações para garantir:
* Integridade dos registros
* Padronização de tipos de dados
* Consistência entre tabelas relacionadas
* Remoção de inconsistências provenientes da API

---

### Version Control
Todo o código-fonte é versionado através da integração entre Databricks e GitHub.
Benefícios:
* Controle de alterações
* Histórico de desenvolvimento
* Colaboração entre desenvolvedores
* Rastreabilidade das mudanças realizadas no pipeline

---

### Lakehouse Platform
O projeto utiliza o Databricks como plataforma Lakehouse, combinando características de Data Lakes e Data Warehouses em uma única arquitetura.
Principais vantagens:
* Armazenamento escalável
* Processamento distribuído com Apache Spark
* Governança centralizada
* Integração com ferramentas analíticas
* Desenvolvimento unificado para Engenharia e Analytics

---

### Future Enhancements
Os próximos passos incluem a implementação de recursos avançados de Engenharia de Dados:
* Camada Gold com métricas analíticas
* Orquestração automatizada dos pipelines
* Monitoramento e observabilidade
* Testes de qualidade de dados
* Dashboards analíticos no Databricks
* Integração com ferramentas externas de visualização
