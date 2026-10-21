# 🏛️ Lakehouse com Databricks e Arquitetura Medalhão

Documentação técnica oficial do **Trabalho 3**, focada na implementação prática de um pipeline de engenharia de dados de ponta a ponta utilizando o **Databricks Free Edition**, **Delta Lake** e a **Arquitetura Medalhão** (Landing ➔ Bronze ➔ Silver ➔ Gold)[cite: 7, 8, 9].

---

## 🎯 Objetivo do Projeto

Construir um pipeline analítico completo no Databricks que extrai dados de bancos relacionais ou não relacionais, persiste de forma bruta, aplica regras rigorosas de qualidade e entrega um modelo dimensional otimizado para Business Intelligence (BI)[cite: 7, 8, 9].

### 🛠️ Especificações Técnicas e Entregas
* **Ambiente**: Databricks Free Edition / Community[cite: 7, 8].
* **Formato de Armazenamento**: Delta Lake (transações ACID)[cite: 8].
* **Orquestração**: Databricks Jobs & Pipelines (execução sequencial).
* **Data de Entrega**: 21/10[cite: 10].

---

## ⚖️ Prós e Contras da Arquitetura Medalhão

| Prós (Vantagens) | Contras (Desafios) |
| :--- | :--- |
| **Desacoplamento Total**: Separação clara entre dados brutos, limpos e agregados. | **Complexidade Operacional**: Exige monitoramento ativo de múltiplos jobs sequenciais. |
| **Rastreabilidade (*Time Travel*)**: Capacidade de auditar e voltar no tempo dos dados no Delta Lake. | **Custo de Processamento**: Transformações pesadas demandam mais recursos do cluster Spark. |
| **Confiabilidade**: Validações de *Data Quality* impedem dados corrompidos na Gold. | **Curva de Aprendizado**: Requer domínio sólido de ecossistemas Spark, Delta e SQL. |

---

## 📋 Sumário da Documentação
* [1. Camada Landing](camadas/landing.md) — Extração das fontes (CSV/JSON).
* [2. Camada Bronze](camadas/bronze.md) — Ingestão para Delta Lake[cite: 8, 9].
* [3. Camada Silver](camadas/silver.md) — Limpeza e *Data Quality*[cite: 8, 9].
* [4. Camada Gold](camadas/gold.md) — Modelagem Dimensional (Ralph Kimball)[cite: 8, 9].
* [5. Automação](orquestracao/jobs.md) — Jobs & Pipelines.
