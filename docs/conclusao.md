# 🏁 Conclusão e Considerações Finais

O desenvolvimento deste **Trabalho 3** permitiu consolidar na prática os principais conceitos da Engenharia de Dados moderna utilizando o ecossistema **Databricks Free Edition**, o formato transacional **Delta Lake** e a estruturação lógica da **Arquitetura Medalhão** (Landing ➔ Bronze ➔ Silver ➔ Gold)[cite: 7, 8, 9].

---

## 🎯 Síntese dos Resultados Obtidos

1. **Extração e Ingestão Eficiente**: Conectámos fontes heterogéneas, garantindo a persistência segura de dados brutos sem perda de fidelidade[cite: 8, 9].
2. **Qualidade Assegurada**: A camada Silver demonstrou a importância de aplicar regras de *Data Quality* rigorosas antes de expor os dados aos utilizadores finais[cite: 8, 9].
3. **Modelagem Orientada a Negócio**: A implementação do *Star Schema* de Ralph Kimball na camada Gold viabilizou consultas analíticas de alta performance para suporte à decisão[cite: 8, 9].
4. **Orquestração Confiável**: O encadeamento via *Jobs & Pipelines* garantiu a autonomia e a previsibilidade na execução sequencial das tarefas[cite: 8, 10].

---

## 📚 Referências Bibliográficas

* **DATABRICKS**. *Introduction to Lakehouse Architecture*. Disponível em: <https://databricks.com/product/lakehouse>. Acesso em: Out. 2026.
* **KIMBALL, Ralph; ROSS, Margy**. *The Data Warehouse Toolkit: The Definitive Guide to Dimensional Modeling*. 3. ed. Indianapolis: John Wiley & Sons, 2013.
* **APACHE SPARK**. *Spark SQL, DataFrames and Datasets Guide*. Disponível em: <https://spark.apache.org/docs/latest/sql-programming-guide.html>. Acesso em: Out. 2026.
