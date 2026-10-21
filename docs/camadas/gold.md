# 🥇 Camada 4: Modelagem Dimensional (GOLD)

A camada **Gold** implementa a modelagem orientada a negócios utilizando o padrão **Star Schema (Ralph Kimball)**, otimizada para ferramentas de BI[cite: 8, 9].

---

## 📐 Estrutura de Modelagem[cite: 8, 9]
* **Dimensões (`dim_`)**: Atributos contextuais (ex: `dim_cliente`, `dim_produto`).
* **Fatos (`fato_`)**: Métricas e chaves estrangeiras transacionais (ex: `fato_vendas`).

```mermaid
erDiagram
    fato_vendas ||--o{ dim_cliente : "possui"
    fato_vendas {
        int id_venda PK
        int id_cliente FK
        date data_venda
        decimal valor_total
    }

## 🛠️ Passo a Passo de Execução

* Ler as tabelas tratadas da camada SILVER[cite: 8, 9, 10].

* Selecionar atributos descritivos para montar as dimensões[cite: 9].

* Cruzar dados para estruturar a tabela fato agregada[cite: 9].

* Persistir as tabelas no schema GOLD[cite: 8, 9].

## 💻 Exemplo Prático de Código (PySpark)

spark.sql("CREATE DATABASE IF NOT EXISTS gold;")

# 1. Criação da Dimensão Cliente
df_cli = spark.read.table("silver.clientes")
dim_cliente = df_cli.select("id_cliente", "nome", "status")
dim_cliente.write.format("delta").mode("overwrite").saveAsTable("gold.dim_cliente")

# 2. Criação da Fato Vendas
df_ped = spark.read.table("silver.pedidos")
fato_vendas = df_ped.select(
    col("id_pedido").alias("id_venda"),
    col("id_cliente"),
    col("data_pedido").alias("data_venda"),
    col("total").alias("valor_total")
)
fato_vendas.write.format("delta").mode("overwrite").saveAsTable("gold.fato_vendas")

print("Modelagem Gold concluída com sucesso!")

## ⚖️ Prós e Contras da Arquitetura Medalhão

| Prós (Vantagens) | Contras (Desafios) |
| :--- | :--- |
| Performance extremamente rápida para consultas analíticas e relatórios (Power BI). | Rigidez estrutural (mudanças exigem alteração na modelagem). |
| Altamente intuitivo para analistas de negócios. | Duplicação otimizada de dados para fins de relatórios. |
