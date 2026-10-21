# 📥 Camada 1: Extração (LANDING / DADOS)

A camada **Landing** é o ponto de contato inicial do pipeline com as fontes de dados do projeto (bancos relacionais ou não relacionais)[cite: 8, 9].

---

## ⚙️ Características Principais
* **Imutabilidade**: Os dados extraídos não sofrem alterações ou tratamentos.
* **Formato de Persistência**: 
  * **CSV**: Utilizado para origens relacionais[cite: 8, 9] (ex: PostgreSQL / Supabase / Render)[cite: 7].
  * **JSON**: Utilizado para origens não relacionais[cite: 8, 9] (ex: MongoDB Atlas)[cite: 7].
* **Schema**: Gravados diretamente no schema `LANDING/DADOS`[cite: 8, 9].

---

## 🛠️ Passo a Passo de Execução
1. Estabelecer conexão via driver JDBC (relacional) ou API de conexão (NoSQL).
2. Varrer as tabelas ou coleções da origem.
3. Executar a leitura criando DataFrames no Spark.
4. Gravar os arquivos brutos no DBFS mantendo a estrutura original[cite: 8, 9].

---

## 💻 Exemplo Prático de Código (PySpark)

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Trabalho3_Landing").getOrCreate()

# Configuração de conexão JDBC para PostgreSQL
jdbc_url = "jdbc:postgresql://[host.render.com:5432/db_trabalho](https://host.render.com:5432/db_trabalho)"
connection_properties = {
    "user": "usuario",
    "password": "senha",
    "driver": "org.postgresql.Driver"
}

tabelas = ["clientes", "pedidos", "produtos"]

for tabela in tabelas:
    print(f"Extraindo tabela da origem: {tabela}")
    df = spark.read.jdbc(url=jdbc_url, table=tabela, properties=connection_properties)
    
    # Gravando em formato CSV no schema Landing
    df.write.mode("overwrite").option("header", "true").csv(f"/dbfs/mnt/landing/{tabela}")
    print(f"Tabela {tabela} persistida na Landing com sucesso!")


## ⚖️ Prós e Contras da Arquitetura Medalhão

| Prós (Vantagens) | Contras (Desafios) |
| :--- | :--- |
| Isola o banco de origem de consultas pesadas de análise. | Ocupa armazenamento duplicado com arquivos brutos textuais. |
| Permite reprocessamento histórico completo a qualquer momento. | **Não valida tipos de dados ou integridade estrutural. |
