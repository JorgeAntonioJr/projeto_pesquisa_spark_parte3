# 📐 Arquitetura do Processamento Spark

Abaixo encontra-se o fluxo de processamento de dados da pesquisa:

```mermaid
graph TD
    A[Dados Brutos / CSV] -->|Leitura PySpark| B(Processamento Spark)
    B -->|Transformações| C[Dataframe Limpo]
    C -->|Persistência| D[(Armazenamento)]
