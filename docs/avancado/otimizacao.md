# ⚡ Otimização, Manutenção e Boas Práticas no Delta Lake

Na arquitetura Lakehouse com Databricks, a gestão eficiente do armazenamento e da computação é fundamental para evitar custos excessivos e garantir alta performance nas consultas analíticas.

---

## 🧹 Compactação de Ficheiros (*OPTIMIZE*)

Com o fluxo contínuo de inserções e atualizações (*Appends* e *Updates*), as tabelas Delta tendem a acumular um grande número de ficheiros pequenos (problema conhecido como *Small File Problem*). Isso degrada drasticamente a performance de leitura do Spark.

### Como funciona o comando OPTIMIZE:
O comando `OPTIMIZE` reorganiza os ficheiros pequenos da tabela Delta em ficheiros maiores e otimizados (geralmente próximos de 1 GB).

```python
# Exemplo de compactação via PySpark SQL em Databricks
spark.sql("OPTIMIZE bronze.pedidos;")
spark.sql("OPTIMIZE silver.pedidos;")
spark.sql("OPTIMIZE gold.fato_vendas;")

## 🗑️ Limpeza de Histórico (VACUUM)

O Delta Lake mantém um histórico de versões antigas dos dados para suportar o Time Travel e transações ACID seguras. Contudo, manter versões obsoletas indefinidamente consome espaço em disco no DBFS.

###Como funciona o comando VACUUM:

O comando VACUUM remove os ficheiros de dados antigos que já não são referenciados pelo log de transações além do período de retenção especificado (por padrão, 7 dias).

```python
# Remove dados obsoletos com retenção de 168 horas (7 dias)
spark.sql("VACUUM bronze.pedidos RETAIN 168 HOURS;")

# Desativar a verificação de segurança para testes (Cuidado em produção!)
spark.conf.set("spark.databricks.delta.vacuum.parallelDelete.enabled", "true")
spark.sql("VACUUM silver.clientes RETAIN 0 HOURS;")

## 🔍 Monitoramento de Performance e Custos

Para garantir que o pipeline execute de forma fluida no ambiente Free Edition / Community do Databricks, recomenda-se:

1. **Utilizar Clusters Single Node:** Evita sobrecarga de comunicação de rede em nós múltiplos para bases de estudo.

2. **Cache de DataFrames:** Aplicar .cache() ou .persist() em DataFrames intermediários que são reutilizados em múltiplas validações da camada Silver.

3. **Estratégias de Z-Ordering:** Aplicar índices de ordenação em colunas de alto filtro (como datas ou IDs de clientes) para acelerar varreduras:

```pyton
spark.sql("OPTIMIZE gold.fato_vendas ZORDER BY (id_cliente, data_venda);")
