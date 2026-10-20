# 📐 Arquitetura e Diagramas

O MkDocs renderiza diagramas de fluxo diretamente a partir de código usando Mermaid.js.

```mermaid
graph TD
    A[Cliente / Navegador] -->|Acessa| B(Servidor Web)
    B -->|Processa| C[Banco de Dados]
    C -->|Retorna dados| B
    B -->|Responde| A
