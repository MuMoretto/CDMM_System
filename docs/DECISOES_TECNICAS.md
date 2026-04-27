# 🎯 Decisões Técnicas - CDMM

Documentação das principais decisões arquiteturais e tecnológicas do projeto.

---

## 1. Por que Python?

### Decisão: Usar Python 3.8+

**Motivos**:
- ✅ **Legibilidade**: Sintaxe clara e intuitiva - ótimo para projetos acadêmicos
- ✅ **Produtividade**: Escrever menos código para fazer mais
- ✅ **Comunidade**: Enorme comunidade e recursos disponíveis
- ✅ **Bibliotecas**: Ecossistema rico (MySQL, web frameworks, data science)
- ✅ **Multiplataforma**: Funciona em Windows, Linux, macOS
- ✅ **Curva de aprendizado**: Ideal para estudantes

---

## 2. Por que MySQL?

### Decisão: Usar MySQL 5.7+ ao invés de PostgreSQL, SQLite, etc.

**Motivos**:
- ✅ **Consolidação em produção**: Amplamente usado em servidores web
- ✅ **Licença**: Open source (GPL), uso livre
- ✅ **Facilidade**: Setup simples e rápido
- ✅ **Performance**: Suficiente para aplicação acadêmica e pequenas empresas
- ✅ **Suporte**: Excelente documentação em português
- ✅ **Ferramentas**: MySQL Workbench para visualização

**Por que não SQLite?**

---

## 3. Por que Padrão MVC?

### Decisão: Arquitetura Model-View-Controller

**Motivos**:
- ✅ **Separação de responsabilidades**: Cada camada com função específica
- ✅ **Manutenibilidade**: Código organizado e fácil de encontrar
- ✅ **Testabilidade**: Cada componente testável isoladamente
- ✅ **Escalabilidade**: Fácil adicionar novos modelos/views
- ✅ **Padrão consolidado**: Usado em frameworks web modernos

**Estrutura**:
```
┌─────────┐
│  VIEW   │ Interface (CLI, Web, Mobile)
└────┬────┘
     │
┌────▼───────────┐
│  CONTROLLER    │ Orquestração de lógica
└────┬───────────┘
     │
┌────▼──────┐
│   MODEL   │ Lógica de negócio
└────┬──────┘
     │
┌────▼────────────┐
│   DATABASE      │ Persistência
└─────────────────┘
```

**Benefícios práticos**:
- Model pode ser usado em API REST depois
- View pode ser substituída por web interface
- Controller fica com lógica reutilizável

---

## 4. Por que Transações ACID?

### Decisão: Usar Context Manager para Transações

```python
with Transaction() as cursor:
    cursor.execute("INSERT...")
    cursor.execute("UPDATE...")
    # Commit automático ao sair do bloco
    # Rollback automático se erro
```

**Motivos**:
- ✅ **Integridade**: Garante que operações relacionadas acontecem juntas
- ✅ **Segurança**: Rollback automático previne inconsistências
- ✅ **Código limpo**: Sem necessidade de commit() manual
- ✅ **Tratamento de erro**: Rollback automático se exceção

**Exemplo real**:
```python
# Sem Transaction (PERIGOSO)
cursor.execute("INSERT INTO pedidos...")
cursor.execute("UPDATE produtos SET estoque = estoque - 1")
# Se erro aqui: pedido inserido mas estoque não atualizado! ❌

# Com Transaction (SEGURO)
with Transaction() as cursor:
    cursor.execute("INSERT INTO pedidos...")
    cursor.execute("UPDATE produtos SET estoque = estoque - 1")
    # Se erro: ambas rollback, nenhuma acontece ✅
```

---

## 5. Por que Prepared Statements?

### Decisão: Usar placeholders %s para SQL

**SEGURO** (Prepared Statements):
```python
cursor.execute("SELECT * FROM usuarios WHERE email = %s", (email,))
```

**INSEGURO** (String Concatenation):
```python
query = f"SELECT * FROM usuarios WHERE email = '{email}'"
cursor.execute(query)
```

**Por quê?**
- ✅ **SQL Injection**: Prepared statements escapam caracteres especiais
- ✅ **Performance**: Query é compilada uma vez
- ✅ **Type Safety**: Tipos de dados são validados

**Exemplo de SQL Injection**:
```python
# Entrada maliciosa
email = "'; DROP TABLE usuarios; --"

# INSEGURO resulta em:
SELECT * FROM usuarios WHERE email = ''; DROP TABLE usuarios; --'

# SEGURO com prepared statement:
SELECT * FROM usuarios WHERE email = '\'; DROP TABLE usuarios; --'
# Tratado como string literal ✅
```

---

## 6. Por que Validação em Múltiplas Camadas?

### Decisão: Validar em Model + Banco de Dados

**Camada 1: Python (Model)**
```python
def salvar(self):
    if not self.email:
        raise ValueError("Email obrigatório")
    if not re.match(r'^[\w\.-]+@[\w\.-]+\.\w+$', self.email):
        raise ValueError("Email inválido")
```

**Camada 2: MySQL (Constraints)**
```sql
CREATE TABLE usuarios (
    email VARCHAR(150) NOT NULL UNIQUE,
    CHECK (CHAR_LENGTH(nome) > 1)
);
```

**Por quê?**
- ✅ **Redundância segura**: Se uma falhar, a outra pega
- ✅ **Performance**: Validação em Python é mais rápida
- ✅ **Segurança**: Banco de dados valida mesmo se alguém acessar diretamente
- ✅ **UX**: Feedback rápido ao usuário (antes de salvar no BD)

---

## 7. Por que Não usar ORM (SQLAlchemy, Django ORM)?

### Decisão: SQL Direto com mysql-connector-python

**SQL Direto**:
```python
cursor.execute("SELECT * FROM usuarios WHERE id = %s", (user_id,))
```

**Com ORM (SQLAlchemy)**:
```python
user = User.query.filter(User.id == user_id).first()
```

**Por que não ORM?**

| Aspecto | ORM | SQL Direto |
|--------|-----|-----------|
| Curva aprendizado | Difícil | Fácil |
| Para projetos pequenos | Overkill | Ideal |
| Controle | Menor | Total |
| Performance | Pode ser lenta | Otimizada |
| Acadêmico | Menos educativo | Mais didático |

**Decisão**: Para projeto acadêmico é melhor SQL direto porque:
- Entende-se realmente como funciona
- Não há complexidade desnecessária
- Ótimo para aprender banco de dados

---

## 8. Por que CLI ao invés de Web Interface?

### Decisão: Interface CLI (Command Line Interface)

**CLI**:
```
========== Sistema CDMM ==========
=  1. Usuários                   =
=  2. Categorias                 =
...
```

**Web Interface**:
```
[Browser] → [Flask/Django] → [MySQL]
```

**Por que CLI agora?**
- ✅ **Simplicidade**: Implementação rápida
- ✅ **Foco**: Lógica de negócio sem distrações com UI
- ✅ **Prototipagem**: Ideal para validar ideias
- ✅ **Requisitos**: Projeto acadêmico sem requisitos web
- ✅ **Testabilidade**: Mais fácil testar lógica em CLI

**Roadmap futuro**:
```
CLI (Atual) → REST API → Web Interface (Flask/React)
```

Modelos podem ser reaproveitados - apenas a camada muda!

---

## 9. Por que .env para Credenciais?

### Decisão: Usar python-dotenv para variáveis de ambiente

```python
# .env (não commitado)
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=senha123

# main.py
import os
from dotenv import load_dotenv
load_dotenv()
host = os.getenv("DB_HOST")
```

**Por quê?**
- ✅ **Segurança**: Credenciais não vão para Git
- ✅ **Flexibilidade**: Mudar config sem mudar código
- ✅ **Ambientes**: Dev, staging, produção diferentes
- ✅ **Boas práticas**: Padrão da indústria

**O que NÃO fazer**:
```python
# ❌ NUNCA fazer isto
DB_PASSWORD = "senha123"  # Vai para Git!

# ❌ Hardcoded
host = "192.168.1.100"    # Não é portável
```

---

## 10. Por que Testes com Pytest?

### Decisão: Usar pytest para testes automatizados

```python
def test_criar_usuario():
    usuario = Usuario("João", "joao@email.com", "11987654321")
    assert usuario.nome == "João"
```

**Por quê?**
- ✅ **Simples**: Sintaxe clara (usar assert direto)
- ✅ **Poderoso**: Fixtures, mocks, parametrização
- ✅ **Comunidade**: Muito usado em projetos Python
- ✅ **CI/CD**: Integra facilmente com GitHub Actions

**Alternativa**: unittest (padrão Python)
- Mais verboso
- Menos usado em projetos modernos

---

**Benefícios**:
- ✅ Histórico claro
- ✅ Automatizar CHANGELOG
- ✅ Versionamento automático (SemVer)

---

**Última atualização**: Abril 2026  
**Revisão por**: Prof. Victor Hugo Braguim Canto
