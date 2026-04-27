# 🏗️ Arquitetura do Sistema CDMM

## Visão Geral

O sistema CDMM utiliza o padrão **Model-View-Controller (MVC)** para separação de responsabilidades, garantindo código limpo, manutenível e escalável.

---

## 📐 Padrão MVC

```
┌─────────────────────────────────────────────────────┐
│                   USUÁRIO (CLI)                     │
└────────────────┬────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│           VIEW (Camada de Apresentação)              │
│  • cdmm_menu.py - Interface CLI                     │
│  • Menus interativos                                │
│  • Input/Output de usuário                          │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│       CONTROLLER (Camada de Controle)                │
│  • cdmm_functions.py - Orquestração                 │
│  • Lógica de fluxo                                  │
│  • Chamadas entre modelos                           │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│        MODEL (Camada de Dados/Negócio)              │
│  • user.py - Usuários                              │
│  • categories.py - Categorias                       │
│  • fornec.py - Fornecedores                         │
│  • products.py - Produtos                           │
│  • orders.py - Pedidos                              │
│  • stock_movement.py - Movimentações               │
│  • Validações de negócio                            │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│         DATABASE (Camada de Persistência)            │
│  • connection.py - Transações MySQL                 │
│  • Schema em banco_de_dados/schema.sql             │
│  • ACID Compliance                                  │
└──────────────────────────────────────────────────────┘
```

---

## 🏢 Estrutura de Pastas

```
CDMM_System/
│
├── 📂 models/                      # Lógica de dados e negócio
│   ├── __init__.py
│   ├── user.py                    # CRUD de usuários
│   ├── categories.py              # CRUD de categorias
│   ├── fornec.py                  # CRUD de fornecedores
│   ├── products.py                # CRUD de produtos
│   ├── orders.py                  # CRUD de pedidos
│   └── stock_movement.py          # CRUD de movimentações
│
├── 📂 views/                       # Interface com usuário
│   ├── __init__.py
│   └── cdmm_menu.py               # Menus CLI
│
├── 📂 controllers/                 # Orquestração de lógica
│   ├── __init__.py
│   └── cdmm_functions.py          # Funções de menu
│
├── 📂 db/                          # Acesso a dados
│   ├── __init__.py
│   └── connection.py              # Transações e conexão
│
├── 📂 reports/                     # Relatórios
│   └── reports.py                 # Queries de análise
│
├── 📂 tests/                       # Testes automatizados
│   ├── conftest.py                # Configuração pytest
│   ├── test_usuario.py
│   ├── test_categories.py
│   ├── test_fornecedor.py
│   ├── test_produto.py
│   ├── test_pedido.py
│   ├── test_stock_movement.py
│   └── test_connection.py
│
├── 📂 database/                    # Scripts SQL
│   └── schema.sql                 # DDL completo
│
├── 📂 docs/                        # Documentação técnica
│   ├── ARQUITETURA.md (este arquivo)
│   ├── BANCO_DADOS.md
│   └── DECISOES_TECNICAS.md
│
├── 📄 main.py                      # Ponto de entrada
├── 📄 README.md                    # Documentação principal
├── 📄 requirements.txt             # Dependências
├── 📄 .gitignore                   # Arquivos ignorados
└── 📄 .env.example                 # Variáveis de ambiente

---

## 🔄 Fluxo de Dados

### Exemplo: Criar um Usuário

```python
# 1. VIEW - Usuário interage via CLI
input("Nome: ")                    # → user.py

# 2. CONTROLLER - cdmm_functions.py orquestra
menu_usuarios()
user = Usuario(nome, email, telefone)

# 3. MODEL - user.py valida e executa lógica
class Usuario:
    def __init__(self, nome, email, telefone):
        self.validar()             # Validação
        
    def salvar(self):
        with Transaction() as cursor:  # 4. DATABASE
            cursor.execute("INSERT INTO...")  # Query

# 4. DATABASE - Executa query
INSERT INTO usuarios (nome, email, telefone) VALUES (...)
```

### Fluxo Visual

```
CLI Input
   ↓
[VIEW] Menu captura entrada
   ↓
[CONTROLLER] Route para função apropriada
   ↓
[MODEL] Instancia classe e valida
   ↓
[DATABASE] Executa transação
   ↓
Resultado retorna ao usuário
```

---

## 🧩 Componentes Principais

### 1. Models (models/)

**Responsabilidade**: Encapsular dados e lógica de negócio

```python
class Usuario:
    def __init__(self, nome, email, telefone):
        self.validar()  # ← Validação em camada
        
    def salvar(self):
        with Transaction() as cursor:
            cursor.execute("INSERT...")
            
    @staticmethod
    def listar():
        with Transaction() as cursor:
            cursor.execute("SELECT...")
            return cursor.fetchall()
```

**Características**:
- ✅ Validações rigorosas de entrada
- ✅ Métodos CRUD (Create, Read, Update, Delete)
- ✅ Transações seguras com banco de dados
- ✅ Tratamento de erros

---

### 2. Controllers (controllers/)

**Responsabilidade**: Orquestração de fluxos e menus

```python
def menu_usuarios():
    while True:
        print("Menu de Usuários")
        opcao = input("Escolha: ")
        
        if opcao == "1":
            nome = input("Nome: ")
            user = Usuario(nome, email, telefone)
            user.salvar()  # ← Chamada ao model
```

**Características**:
- ✅ Lógica de fluxo de aplicação
- ✅ Chamadas aos modelos
- ✅ Tratamento de inputs
- ✅ Exibição de feedback ao usuário

---

### 3. Views (views/)

**Responsabilidade**: Apresentação e interação com usuário

```python
def menu_principal():
    while True:
        os.system('cls')  # Limpar tela
        print("========== Sistema CDMM ==========")
        print("=  1. Usuários                   =")
        # ... mais opções
        opcao = input("\nEscolha uma opção: ")
```

**Características**:
- ✅ Interface CLI limpa
- ✅ Menus estruturados
- ✅ Sem lógica de negócio
- ✅ Input/Output simples

---

### 4. Database Layer (db/)

**Responsabilidade**: Conexão e transações seguras

```python
class Transaction:
    def __enter__(self):
        self.conn = get_connection()
        self.cursor = self.conn.cursor(dictionary=True)
        self.conn.start_transaction()
        return self.cursor
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is None:
            self.conn.commit()
        else:
            self.conn.rollback()
```

**Características**:
- ✅ Context Manager para segurança
- ✅ Commit/Rollback automático
- ✅ ACID Compliance
- ✅ Connection pooling

---

### 5. Reports (reports/)

**Responsabilidade**: Queries complexas e análises

```python
def r_estoque_por_categoria():
    """Gera relatório de estoque agrupado por categoria"""
    query = """
        SELECT c.nome, SUM(p.quantidade_estoque)
        FROM produtos p
        JOIN categorias c ON p.id_categoria = c.id
        GROUP BY c.nome
    """
```

---

## 🔐 Segurança da Arquitetura

### 1. Validações em Camadas

```
CLI Input → MODEL Validation → DATABASE Constraints
```

Exemplo:
```python
# MODEL valida
if not re.match(r'^[\w\.-]+@[\w\.-]+\.\w+$', self.email):
    raise ValueError("Email inválido")

# DATABASE reforça
CREATE TABLE usuarios (
    email VARCHAR(150) NOT NULL UNIQUE
)
```

### 2. Transações ACID

```python
with Transaction() as cursor:
    cursor.execute("INSERT INTO usuarios...")
    cursor.execute("INSERT INTO historico...")
    # Se erro: rollback automático
    # Se sucesso: commit automático
```

### 3. Prepared Statements (SQL Injection)

```python
# ✅ SEGURO - Prepared statement
cursor.execute("SELECT * FROM usuarios WHERE email = %s", (email,))

# ❌ INSEGURO - String concatenation
query = f"SELECT * FROM usuarios WHERE email = '{email}'"
```

### 4. Variáveis de Ambiente

```python
# .env (NÃO commitado no Git)
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=senha_segura

# Código acessa via dotenv
os.getenv("DB_PASSWORD")  # Nunca hardcodado
```

---

## 📊 Diagrama de Entidades

```
┌──────────────┐
│  USUARIOS    │
├──────────────┤
│ id (PK)      │
│ nome         │
│ email (U)    │
│ telefone (U) │
└──────────────┘
       │
       │ 1:N
       │
       ▼
┌──────────────┐
│   PEDIDOS    │
├──────────────┤
│ id (PK)      │
│ id_usuario   │
│ total_pedido │
│ status       │
└──────────────┘
       │
       │ 1:N
       │
       ▼
┌──────────────────┐      ┌──────────────┐
│  ITENS_PEDIDO    │      │  PRODUTOS    │
├──────────────────┤      ├──────────────┤
│ id (PK)          │─────→│ id (PK)      │
│ id_pedido (FK)   │ N:1  │ nome         │
│ id_produto (FK)  │      │ sku (U)      │
│ quantidade       │      │ preco        │
│ subtotal         │      │ estoque      │
└──────────────────┘      └──────────────┘
                                 │
                          1:N    │
                                 │
                ┌────────────────┤
                │                │
        ┌───────▼──────┐  ┌──────▼──────────┐
        │ CATEGORIAS   │  │ FORNECEDORES    │
        ├──────────────┤  ├─────────────────┤
        │ id (PK)      │  │ id (PK)         │
        │ nome (U)     │  │ nome (U)        │
        │ descricao    │  │ contato         │
        └──────────────┘  └─────────────────┘
```

---

## 🔧 Padrões de Design Utilizados

### 1. **Context Manager** (Transações)
```python
with Transaction() as cursor:
    # Automaticamente faz commit se tudo OK
    # Faz rollback se houver erro
```

### 2. **Singleton** (Connection)
```python
def get_connection():
    # Reutiliza conexão existente
```

### 3. **Repository Pattern** (Models)
```python
class Usuario:
    @staticmethod
    def listar()  # SELECT
    def salvar()  # INSERT
    @staticmethod
    def editar()  # UPDATE
    @staticmethod
    def excluir()  # DELETE
```

---

## 🎯 Princípios SOLID

### S - Single Responsibility
- Cada classe tem uma responsabilidade
- Usuario.py → apenas usuários
- Connection.py → apenas conexão

### O - Open/Closed
- Classes abertas para extensão
- Fechadas para modificação

### L - Liskov Substitution
- Models seguem contrato comum

### I - Interface Segregation
- Interfaces mínimas e específicas

### D - Dependency Inversion
- Depende de abstrações
- Transaction é abstração de conexão

---

**Última atualização**: Abril 2026  
**Versão da Arquitetura**: 1.0
