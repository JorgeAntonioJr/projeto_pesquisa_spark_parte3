# ⚙️ Automação e Orquestração (Jobs & Pipelines)

Para garantir que o fluxo do Lakehouse rode de maneira automatizada e sequencial, utilizamos os recursos de **Jobs & Pipelines** do Databricks.

---

## 🔄 Fluxo de Dependências das Tasks[cite: 8, 10]

```mermaid
graph TD
    T1[Task 1: Landing / Extração] -->|Sucesso| T2[Task 2: Bronze / Ingestão]
    T2 -->|Sucesso| T3[Task 3: Silver / Data Quality]
    T3 -->|Sucesso| T4[Task 4: Gold / Modelagem Kimball]

## 🛠️ Passo a Passo para Configuração no Databricks

* No painel lateral, selecione Workflows[cite: 8, 10].

* Clique em Create Job[cite: 8, 10].

* Adicione as 4 tasks encadeando os notebooks correspondentes às camadas[cite: 8, 10].

* Configure o agendamento ou execute manualmente clicando em Run Now[cite: 8, 10].

## ⚖️ Prós e Contras da Arquitetura Medalhão

| Prós (Vantagens) | Contras (Desafios) |
| :--- | :--- |
| Automação completa do pipeline sem ferramentas externas adicionais. | Funcionalidades de governança e monitoramento avançado são limitadas na versão Free. |
| Tratamento nativo de falhas e reexecuções de tarefas individuais. | Dependência da infraestrutura do Databricks Community. |
