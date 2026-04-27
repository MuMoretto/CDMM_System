# 📦 CDMM - Sistema de Gerenciamento de Estoque

Um sistema completo e robusto de gerenciamento de estoque desenvolvido em Python, com interface CLI intuitiva e banco de dados relacional MySQL.

**Trabalho Acadêmico** - Curso de Ciência da Computação | UNISAGRADO | Bauru - SP

## 🎯 Visão Geral

O **CDMM (Central de Distribuição e Gerenciamento de Materiais)** é um sistema empresarial projetado para gerenciar de forma integrada todos os aspectos de um sistema de estoque, desde o cadastro de produtos até a geração de relatórios analíticos.

---

## ✨ Características Principais

- ✅ **Gestão de Usuários** - Cadastro, edição e exclusão de usuários do sistema
- ✅ **Categorização de Produtos** - Organização hierárquica de produtos por categoria
- ✅ **Gerenciamento de Fornecedores** - Cadastro e manutenção de dados de fornecedores
- ✅ **Controle de Produtos** - CRUD completo com SKU, preço e estoque
- ✅ **Pedidos** - Criação, acompanhamento e gerenciamento de pedidos
- ✅ **Movimentação de Estoque** - Rastreamento de entradas, saídas e ajustes
- ✅ **Relatórios Avançados** - Consultas analíticas sobre estoque, pedidos e performance
- ✅ **Validação de Dados** - Validações rigorosas em entrada de dados
- ✅ **Transações ACID** - Integridade garantida das operações no banco de dados

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Propósito |
|-----------|--------|----------|
| **Python** | 3.8+ | Linguagem principal |
| **MySQL** | 5.7+ | Banco de dados relacional |
| **mysql-connector-python** | 8.0+ | Driver de conexão MySQL |
| **python-dotenv** | 0.19+ | Gerenciamento de variáveis de ambiente |

---

## 📋 Pré-requisitos

Antes de começar, certifique-se de ter instalado:

1. **Python 3.8+** - [Download](https://www.python.org/downloads/)
2. **MySQL Server** - [Download](https://downloads.mysql.com/archives/community/)
3. **MySQL Workbench** (opcional) - [Download](https://dev.mysql.com/downloads/workbench/)
4. **pip** - Gerenciador de pacotes Python (geralmente incluído com Python)

---

## 🚀 Instalação e Configuração

### 1. Criar Ambiente Virtual

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/MacOS
python3 -m venv venv
source venv/bin/activate
```

### 2. Instalar Dependências

```bash
pip install -r requirements.txt
```

Ou instale manualmente:

```bash
pip install mysql-connector-python python-dotenv
```

### 3. Configurar Banco de Dados

#### Criar o Banco de Dados

Abra o MySQL Workbench ou terminal MySQL e execute:

```bash
mysql -u root -p < script_bd_cdmm.md
```

Ou copie e execute manualmente o script em `script_bd_cdmm.md`

#### Configurar Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=sua_senha_mysql
DB_NAME=sistema_cdmm
```

### 4. Executar o Sistema

```bash
python main.py
```

---

## 📖 Como Usar

Após executar `python main.py`, você verá o menu principal:

```
========== Sistema CDMM ==========
=  1. Usuários                   =
=  2. Categorias                 =
=  3. Fornecedores               =
=  4. Produtos                   =
=  5. Pedidos                    =
=  6. Movimentações de Estoque   =
=  7. Relatórios de Consulta     =
=  0. Sair                        =
==================================
```

### Exemplos de Uso:

#### 1️⃣ Gerenciar Usuários
- Cadastrar novo usuário com validação de email e telefone
- Listar todos os usuários cadastrados
- Editar dados de usuário existente
- Excluir usuário do sistema

#### 2️⃣ Gerenciar Categorias
- Criar novas categorias de produtos
- Visualizar todas as categorias
- Editar nome e descrição
- Excluir categorias
- Editar descrição de forma independente

#### 3️⃣ Gerenciar Fornecedores
- Cadastrar fornecedores com nome e contato
- Listar fornecedores disponíveis
- Editar informações de contato
- Excluir fornecedores

#### 4️⃣ Gerenciar Produtos
- Cadastrar produtos com SKU único
- Associar a categorias e fornecedores
- Definir preço e quantidade inicial
- Ativar/desativar produtos
- Consultar status do estoque

#### 5️⃣ Gerenciar Pedidos
- Criar novos pedidos para usuários
- Adicionar produtos aos pedidos
- Acompanhar status (em andamento, concluído, cancelado)
- Consultar histórico de pedidos

#### 6️⃣ Movimentação de Estoque
- Registrar entradas de produtos
- Registrar saídas
- Fazer ajustes de estoque
- Rastrear referências (NF, romaneio, etc.)

#### 7️⃣ Relatórios
- **Estoque por Categoria**: Visualiza quantidade e valor total por categoria
- **Pedidos por Fornecedor**: Analisa volume e valor de pedidos por fornecedor
- **Produtos sem Estoque**: Identifica produtos com zero quantidade
- **Produtos Mais Vendidos**: Ranking dos produtos com maior saída

---

## 📁 Estrutura do Projeto

```
CDMM_System/
├── main.py                      # Ponto de entrada da aplicação
├── script_bd_cdmm.md           # Script SQL para criar banco de dados
├── requirements.txt             # Dependências Python
├── .env                        # Variáveis de ambiente (não incluído)
│
├── db/                         # Módulo de banco de dados
│   ├── __init__.py
│   └── connection.py           # Conexão e transações com MySQL
│
├── models/                     # Modelos de dados e ORM
│   ├── __init__.py
│   ├── user.py                # Modelo de Usuário
│   ├── categories.py          # Modelo de Categorias
│   ├── fornec.py              # Modelo de Fornecedores
│   ├── products.py            # Modelo de Produtos
│   ├── orders.py              # Modelo de Pedidos
│   └── stock_movement.py      # Modelo de Movimentações
│
├── controllers/               # Controladores e lógica de negócio
│   ├── __init__.py
│   └── cdmm_functions.py      # Funções de controle dos menus
│
├── views/                     # Camada de apresentação
│   ├── __init__.py
│   └── cdmm_menu.py           # Interface CLI do sistema
│
├── reports/                   # Módulo de relatórios
│   └── reports.py             # Funções de geração de relatórios
│
└── tests/                     # Suite de testes
    ├── __init__.py
    ├── conftest.py            # Configuração de testes
    ├── test_connection.py      # Testes de conexão
    ├── test_usuario.py         # Testes de usuário
    ├── test_produto.py         # Testes de produtos
    ├── test_pedido.py          # Testes de pedidos
    ├── test_forecedor.py       # Testes de fornecedores
    └── test_stock_movement.py # Testes de movimentações
```

---

## 🗄️ Estrutura do Banco de Dados

### Tabelas Principais

#### `usuarios`
```sql
id INT (PK) | nome VARCHAR(150) | email VARCHAR(150) | telefone VARCHAR(30) | criado_em DATETIME
```

#### `categorias`
```sql
id INT (PK) | nome VARCHAR(100) | descricao VARCHAR(255) | criado_em DATETIME
```

#### `fornecedores`
```sql
id INT (PK) | nome VARCHAR(150) | contato VARCHAR(150)
```

#### `produtos`
```sql
id INT (PK) | nome VARCHAR(150) | sku VARCHAR(50) | id_categoria INT (FK) | 
id_fornecedor INT (FK) | preco DECIMAL(12,2) | quantidade_estoque INT | 
ativo BOOLEAN | criado_em DATETIME
```

#### `pedidos`
```sql
id INT (PK) | id_usuario INT (FK) | data_pedido DATETIME | total_pedido DECIMAL(12,2) | 
status ENUM('em_andamento','concluido','cancelado') | observacao VARCHAR(255)
```

#### `itens_pedido`
```sql
id INT (PK) | id_pedido INT (FK) | id_produto INT (FK) | quantidade INT | 
preco_unitario DECIMAL(12,2) | subtotal DECIMAL(12,2)
```

#### `estoque_movimentacoes`
```sql
id INT (PK) | id_produto INT (FK) | tipo_movimentacao ENUM('entrada','saida','ajuste') | 
quantidade INT | data_movimentacao DATETIME | referencia VARCHAR(100) | observacao VARCHAR(255)
```

### Índices e Views
- Índices para otimização de buscas
- View `vw_pedidos_resumo` para consultas rápidas

---

## 🧪 Testes

O projeto inclui uma suite de testes automatizados. Para executar:

```bash
# Instalar pytest
pip install pytest pytest-mysql

# Executar todos os testes
pytest

# Executar teste específico
pytest tests/test_usuario.py

# Executar com verbose
pytest -v
```

---

## ✅ Validações de Dados

### Usuários
- ✓ Nome obrigatório com pelo menos uma letra
- ✓ Email válido e único
- ✓ Telefone com 11 dígitos (com DDD)

### Categorias
- ✓ Nome obrigatório e único
- ✓ Descrição opcional

### Fornecedores
- ✓ Nome obrigatório com apenas letras
- ✓ Contato obrigatório

### Produtos
- ✓ SKU único
- ✓ Preço >= 0
- ✓ Quantidade >= 0
- ✓ Categoria e Fornecedor opcionais

### Pedidos
- ✓ Total >= 0
- ✓ Status validado
- ✓ Usuário válido

---

## 🔒 Segurança

- **Transações ACID**: Garante integridade das operações
- **Prepared Statements**: Prevenção contra SQL Injection
- **Validação de Entrada**: Todos os inputs são validados
- **Variáveis de Ambiente**: Credenciais não expostas no código
- **Constraints de Banco**: Validações em nível de banco de dados

---

## 📊 Relatórios Disponíveis

1. **Estoque por Categoria**
   - Total de quantidade em estoque
   - Valor total em R$ por categoria

2. **Pedidos por Fornecedor**
   - Total de pedidos
   - Valor total de pedidos

3. **Produtos sem Estoque**
   - Lista de produtos com quantidade = 0
   - Informações detalhadas

4. **Produtos Mais Vendidos**
   - Ranking de saídas
   - Análise de performance

---

## 🐛 Troubleshooting

### Erro de Conexão com Banco de Dados
```
[ERRO] Falha na conexão com o banco de dados
```
- Verifique se MySQL está rodando
- Confirme as credenciais no arquivo `.env`
- Verifique se o banco `sistema_cdmm` existe

### Módulo não encontrado
```
ModuleNotFoundError: No module named 'mysql'
```
- Execute: `pip install mysql-connector-python`

### Erro de Variáveis de Ambiente
```
KeyError: 'DB_HOST'
```
- Crie arquivo `.env` com as variáveis necessárias
- Verifique se o arquivo está na raiz do projeto

---

## 📝 Padrões de Código

- **Padrão MVC**: Model-View-Controller separado
- **Context Manager**: Uso de `with` para transações
- **Validação em Camadas**: Validação em model e banco
- **Exception Handling**: Tratamento robusto de erros

---

## 📄 Licença

Este projeto é desenvolvido para fins educacionais e empresariais.

---

## 👨‍💻 Equipe de Desenvolvimento

### 👥 Membros da Equipe

| Nome | Função |
|------|--------|
| **Carlos Eduardo Rodrigues Silva** | Desenvolvedor |
| **Daniel Lucarelli Cerri** | Desenvolvedor |
| **Melck Silva de Oliveira Nascimento** | Desenvolvedor |
| **Murilo Moretto Marques** | Desenvolvedor |

### 🎓 Orientação Acadêmica

- **Orientador**: Prof. Victor Hugo Braguim Canto
- **Instituição**: UNISAGRADO - Universidade Sagrado Coração
- **Campus**: Bauru - SP
- **Curso**: Ciência da Computação
- **Tipo**: Trabalho Acadêmico

---

## 📞 Suporte

Para dúvidas ou problemas, verifique:
- [Documentação MySQL](https://dev.mysql.com/doc/)
- [Documentação Python](https://docs.python.org/3/)
- [Arquivo importante.md](importante.md) - Referências adicionais

---

**Versão**: 1.0.0  
**Última atualização**: Abril de 2026
