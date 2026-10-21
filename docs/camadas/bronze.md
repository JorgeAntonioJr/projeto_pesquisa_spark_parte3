# 🥉 Camada 2: Ingestão (BRONZE)

A camada **Bronze** consome os arquivos brutos da `Landing` e os converte para o formato padrão do Delta Lake[cite: 8, 9].

---

## ⚙️ Características Principais
* **Formato Colunar (Delta)**: Alta performance e segurança em transações ACID.
* **Histórico Preservado**: Suporte nativo a *Time Travel*.
* **Schema Evolution**: Capacidade de absorver pequenas mudanças estruturais.

---

## 🛠️ Passo a Passo de Execução
1. Ler os arquivos CSV/JSON extraídos na camada `Landing`.
2. Inferir o schema de dados inicial.
3. Gravar como tabelas gerenciadas no schema `BRONZE` usando o formato `.format("delta")`[cite: 8, 9].

---

## 💻 Exemplo Prático de Código (PySpark)

```python
tabelas = ["clientes", "pedidos", "produtos"]

spark.sql("CREATE DATABASE IF NOT EXISTS bronze;")

for tabela in tabelas:
    print(f"Ingerindo {tabela} para o Delta Lake...")
    
    # Leitura dos dados brutos
    df_raw = spark.read.option("header", "true").option("inferSchema", "true").csv(f"/dbfs/mnt/landing/{tabela}")
    
    # Gravação no schema Bronze em formato Delta
    df_raw.write.format("delta").mode("overwrite").saveAsTable(f"bronze.{tabela}")
    print(f"Tabela bronze.{tabela} criada com sucesso!")


## ⚖️ Prós e Contras da Arquitetura Medalhão

| Prós (Vantagens) | Contras (Desafios) |
| :--- | :--- |
| Performance de leitura muito superior comparada a CSV/JSON puros. | Os dados ainda contêm inconsistências operacionais e duplicatas. |
| Confiabilidade de transações em nuvem. | Requer manutenção periódica de arquivos (OPTIMIZE). |
