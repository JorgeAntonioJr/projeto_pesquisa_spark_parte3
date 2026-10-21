# 🥈 Camada 3: Data Quality e Tratamento (SILVER)

A camada **Silver** aplica regras avançadas de **Data Quality** e padronização, entregando dados limpos e confiáveis[cite: 8, 9].

---

## 🛡️ Regras de Data Quality Aplicadas
1. **Deduplicação**: Eliminação de registros duplicados (`dropDuplicates`).
2. **Validação de Nulos**: Tratamento de campos obrigatórios (`NotNull`).
3. **Padronização Textual**: Conversão para maiúsculas e remoção de espaços em branco (`trim`, `upper`).
4. **Casting de Tipos**: Ajustes de tipos numéricos e temporais.

---

## 🛠️ Passo a Passo de Execução
1. Ler as tabelas Delta do schema `BRONZE`.
2. Aplicar funções de transformação e filtros lógicos[cite: 9].
3. Persistir o resultado no schema `SILVER` em formato Delta Lake[cite: 8, 9].

---

## 💻 Exemplo Prático de Código (PySpark)

```python
from pyspark.sql.functions import col, trim, upper, current_timestamp

spark.sql("CREATE DATABASE IF NOT EXISTS silver;")

print("Processando Data Quality na tabela Clientes...")
df_bronze = spark.read.table("bronze.clientes")

df_silver = df_bronze \
    .dropDuplicates(["id_cliente"]) \
    .filter(col("id_cliente").isNotNull()) \
    .withColumn("nome", upper(trim(col("nome")))) \
    .withColumn("data_processamento", current_timestamp())

# Gravando na camada Silver
df_silver.write.format("delta").mode("overwrite").saveAsTable("silver.clientes")
print("Tabela silver.clientes processada com sucesso!")

## ⚖️ Prós e Contras da Arquitetura Medalhão

| Prós (Vantagens) | Contras (Desafios) |
| :--- | :--- |
| Garante dados limpos e prontos para cruzamentos analíticos. | Exige regras complexas de mapeamento de exceções e regras de negócio. |
| Base unificada confiável para a camada de negócios. | Requer monitoramento contínuo para evitar descarte incorreto de registros. |
