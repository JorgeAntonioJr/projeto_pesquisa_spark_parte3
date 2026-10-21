# 🔒 Governança, Segurança e Gestão de Segredos

Em ambientes de Engenharia de Dados modernos, a segurança da informação é um pilar crítico. Expor credenciais de acesso a bases de dados diretamente no código fonte é uma falha grave de projeto (*hardcoding*).

---

## 🛡️ Utilização do Databricks Secrets (`dbutils.secrets`)

Para mitigar riscos de segurança no **Trabalho 3**, as credenciais de acesso às origens relacionais (como PostgreSQL no Supabase/Render) e não relacionais (MongoDB Atlas) foram isoladas utilizando o gestor de segredos nativo do Databricks.

### Como funciona na prática:
Em vez de escrever a senha em texto plano, o notebook consome a chave de forma encriptada em tempo de execução:

```python
# Recuperando credenciais de forma segura via DBUtils
db_user = dbutils.secrets.get(scope = "trabalho3-scope", key = "db-user")
db_password = dbutils.secrets.get(scope = "trabalho3-scope", key = "db-password")

# Atribuindo propriedades de conexão JDBC
connection_properties = {
    "user": db_user,
    "password": db_password,
    "driver": "org.postgresql.Driver"
}

print("Credenciais carregadas com segurança através do escopo de segredos do Databricks.")

## 👥 Controlo de Acessos por Camadas (ACLs)

Na arquitetura Lakehouse implementada, a segurança e a governança são aplicadas por nível de maturidade dos dados:

* **Camada Landing / Bronze:** Acesso restrito apenas a engenheiros de dados e administradores do pipeline.

* **Camada Silver / Gold:** Acesso liberado para analistas de dados, ferramentas de BI e cientistas de dados, garantindo que utilizem apenas dados tratados e validados.
