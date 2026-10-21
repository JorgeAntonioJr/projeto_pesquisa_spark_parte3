# 📈 Consultas SQL Analíticas (Camada Gold)

A camada **Gold**, estruturada no padrão dimensional de Ralph Kimball, permite que analistas de negócios e ferramentas de BI executem consultas complexas de forma direta, sem a necessidade de joins pesados ou limpezas prévias.

Abaixo estão exemplos práticos de consultas SQL analíticas executadas sobre as tabelas do schema `gold`.

---

## 1. Faturamento Total por Categoria de Produto
Esta query cruza a `fato_vendas` com a `dim_produto` para calcular a receita bruta gerada por categoria.

```sql
SELECT 
    p.categoria,
    COUNT(f.id_venda) AS total_vendas,
    SUM(f.quantidade) AS quantidade_total_itens,
    ROUND(SUM(f.valor_total), 2) AS faturamento_total
FROM 
    gold.fato_vendas f
JOIN 
    gold.dim_produto p ON f.id_produto = p.id_produto
GROUP BY 
    p.categoria
ORDER BY 
    faturamento_total DESC;

## 2. Análise de Comportamento de Clientes Ativos vs Inativos
Consulta para avaliar o volume de transações e o ticket médio agrupado pelo status cadastral do cliente na dim_cliente.

```sql
SELECT 
    c.status AS status_cliente,
    COUNT(DISTINCT c.id_cliente) AS qtd_clientes,
    COUNT(f.id_venda) AS total_pedidos,
    ROUND(AVG(f.valor_total), 2) AS ticket_medio,
    ROUND(SUM(f.valor_total), 2) AS receita_gerada
FROM 
    gold.fato_vendas f
JOIN 
    gold.dim_cliente c ON f.id_cliente = c.id_cliente
GROUP BY 
    c.status;

## 3. Top 10 Clientes com Maior Volume de Compras
Identificação dos clientes mais valiosos (Lifetime Value) para campanhas de fidelização.

```sql
SELECT 
    c.id_cliente,
    c.nome_cliente,
    COUNT(f.id_venda) AS total_compras,
    SUM(f.valor_total) as valor_gasto_acumulado
FROM 
    gold.fato_vendas f
JOIN 
    gold.dim_cliente c ON f.id_cliente = c.id_cliente
GROUP BY 
    c.id_cliente,
    c.nome_cliente
ORDER BY 
    valor_gasto_acumulado DESC
LIMIT 10;
