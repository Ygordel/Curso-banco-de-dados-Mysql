# 📚 Aula 01 — Fundamentos de Banco de Dados com MySQL

**Curso:** Banco de Dados com MySQL  
**Aula:** 01  
**Tema:** Criação de banco, tabelas e consultas SQL  
**Professor:** Ygor Silva  
**Tecnologia:** MySQL  

---

## 🎯 Objetivos da aula

Nesta aula serão praticados os comandos básicos para:

- visualizar bancos de dados;
- criar e selecionar um banco de dados;
- criar tabelas;
- consultar a estrutura de uma tabela;
- inserir registros;
- realizar consultas com `SELECT`;
- ordenar resultados com `ORDER BY`;
- limitar resultados com `LIMIT`;
- realizar operações matemáticas;
- utilizar aliases com `AS`.

---

## 🧠 1. Conceitos básicos

### Banco de dados

Um banco de dados organiza os objetos utilizados para armazenar e manipular informações.

### Tabela

Uma tabela é formada por campos e registros, sendo utilizada para armazenar os dados.

---

# 🛠️ 2. Criando o banco de dados

### Visualizar os bancos existentes

```sql
SHOW DATABASES;
```

### Criar o banco

```sql
CREATE DATABASE aula1;
```

### Selecionar o banco

```sql
USE aula1;
```

### Excluir um banco

> ⚠️ **Cuidado:** `DROP DATABASE` remove o banco de dados e seus objetos.

```sql
DROP DATABASE aula1;
```

---

# 🗂️ 3. Criando a tabela `produto`

A tabela utilizada nesta aula possui quatro campos:

| Campo | Tipo | Finalidade |
|---|---|---|
| `nome` | `VARCHAR(40)` | Nome do produto |
| `fabric` | `VARCHAR(30)` | Fabricante |
| `qtd` | `INT` | Quantidade |
| `preco` | `FLOAT` | Preço |

### Comando

```sql
CREATE TABLE produto(
    nome VARCHAR(40),
    fabric VARCHAR(30),
    qtd INT,
    preco FLOAT
);
```

---

# 🔎 4. Consultando tabelas

### Exibir as tabelas do banco

```sql
SHOW TABLES;
```

### Exibir a estrutura da tabela

```sql
DESC produto;
```

---

# ➕ 5. Inserindo dados

```sql
INSERT INTO produto VALUES('Notebook', 'Dell', 2, 2800);
INSERT INTO produto VALUES('Impressora', 'HP', 3, 750);
INSERT INTO produto VALUES('Projetor', 'Epson', 1, 2300);
INSERT INTO produto VALUES('Monitor', 'LG', 4, 650.5);
INSERT INTO produto VALUES('Camera', 'Sony', 2, 899.99);
INSERT INTO produto VALUES('Computador', 'Dell', 1, 1750);
INSERT INTO produto VALUES('Smartphone', 'Sony', 2, 2100);
```

---

# 🔍 6. Consultando os registros

A estrutura básica de uma consulta é:

```sql
SELECT lista_de_campos
FROM nome_da_tabela;
```

### Consultar nome e preço

```sql
SELECT nome, preco FROM produto;
```

### Consultar nome, quantidade e preço

```sql
SELECT nome, qtd, preco FROM produto;
```

### Alterar a ordem das colunas

```sql
SELECT fabric, nome, preco FROM produto;
```

### Consultar todas as colunas

```sql
SELECT nome, fabric, qtd, preco FROM produto;
```

Também podemos utilizar:

```sql
SELECT * FROM produto;
```

---

# ↕️ 7. Ordenando resultados com `ORDER BY`

O comando `ORDER BY` permite definir a ordem dos registros apresentados no resultado.

### Ordenar pelo nome

```sql
SELECT nome, preco
FROM produto
ORDER BY nome;
```

### Ordenar pelo fabricante

```sql
SELECT fabric, nome, preco
FROM produto
ORDER BY fabric;
```

### Ordenar pela quantidade

```sql
SELECT nome, qtd, preco
FROM produto
ORDER BY qtd;
```

### Ordenar pelo preço

```sql
SELECT nome, qtd, preco
FROM produto
ORDER BY preco;
```

### Ordem decrescente

```sql
SELECT nome, qtd, preco
FROM produto
ORDER BY preco DESC;
```

### Ordenação por mais de um campo

```sql
SELECT fabric, nome, preco
FROM produto
ORDER BY fabric, nome;
```

### Misturando ordem crescente e decrescente

```sql
SELECT fabric, nome, preco
FROM produto
ORDER BY fabric ASC, preco DESC;
```

---

# 🔢 8. Limitando resultados com `LIMIT`

O comando `LIMIT` permite restringir a quantidade de registros exibidos.

### Primeiros 5 registros

```sql
SELECT nome, qtd, preco
FROM produto
LIMIT 5;
```

### Três produtos com menor preço

```sql
SELECT nome, qtd, preco
FROM produto
ORDER BY preco
LIMIT 3;
```

### Três produtos com maior preço

```sql
SELECT nome, qtd, preco
FROM produto
ORDER BY preco DESC
LIMIT 3;
```

### Sintaxe com início e quantidade

```sql
SELECT nome, qtd, preco
FROM produto
LIMIT 3, 2;
```

Outro exemplo:

```sql
SELECT nome, qtd, preco
FROM produto
LIMIT 5, 2;
```

---

# ➗ 9. Operações matemáticas

O SQL também permite realizar cálculos diretamente nas consultas.

### Soma

```sql
SELECT nome, qtd, qtd + 5
FROM produto;
```

### Subtração

```sql
SELECT nome, preco, preco - 100
FROM produto;
```

### Multiplicação

```sql
SELECT nome, qtd, qtd * 2
FROM produto;
```

### Divisão

```sql
SELECT nome, preco, preco / 2
FROM produto;
```

### Quantidade × preço

```sql
SELECT nome, qtd, preco, qtd * preco
FROM produto;
```

---

# 🏷️ 10. Utilizando `AS` — Alias

O `AS` permite alterar o rótulo apresentado para uma coluna no resultado da consulta.

### Exemplo

```sql
SELECT
    nome AS NomeProduto,
    qtd,
    preco,
    qtd * preco AS Total
FROM produto;
```

### Calculando desconto

```sql
SELECT
    nome,
    preco,
    preco * (4.5 / 100) AS Desconto,
    preco - (preco * (4.5 / 100)) AS PrecoDesconto
FROM produto;
```

---

# 🧪 11. Laboratório completo

O roteiro abaixo pode ser executado no MySQL para reproduzir a prática da aula.

```sql
-- Criar o banco
CREATE DATABASE aula1;

-- Selecionar o banco
USE aula1;

-- Criar a tabela
CREATE TABLE produto(
    nome VARCHAR(40),
    fabric VARCHAR(30),
    qtd INT,
    preco FLOAT
);

-- Inserir os registros
INSERT INTO produto VALUES('Notebook', 'Dell', 2, 2800);
INSERT INTO produto VALUES('Impressora', 'HP', 3, 750);
INSERT INTO produto VALUES('Projetor', 'Epson', 1, 2300);
INSERT INTO produto VALUES('Monitor', 'LG', 4, 650.5);
INSERT INTO produto VALUES('Camera', 'Sony', 2, 899.99);
INSERT INTO produto VALUES('Computador', 'Dell', 1, 1750);
INSERT INTO produto VALUES('Smartphone', 'Sony', 2, 2100);

-- Consultar os dados
SELECT * FROM produto;

-- Ordenar pelo preço
SELECT nome, qtd, preco
FROM produto
ORDER BY preco DESC;

-- Limitar o resultado
SELECT nome, qtd, preco
FROM produto
ORDER BY preco DESC
LIMIT 3;

-- Calcular o total por produto
SELECT
    nome,
    qtd,
    preco,
    qtd * preco AS Total
FROM produto;

-- Calcular desconto de 4,5%
SELECT
    nome,
    preco,
    preco * (4.5 / 100) AS Desconto,
    preco - (preco * (4.5 / 100)) AS PrecoDesconto
FROM produto;
```

---

# 📝 12. Atividade prática

Após executar o laboratório, pratique os seguintes comandos:

1. Exiba todos os bancos de dados existentes.
2. Selecione o banco `aula1`.
3. Liste as tabelas existentes.
4. Mostre a estrutura da tabela `produto`.
5. Exiba somente `nome` e `fabric`.
6. Liste os produtos pelo maior preço.
7. Exiba somente os três produtos mais caros.
8. Calcule o valor total de cada produto utilizando `qtd * preco`.
9. Crie um alias chamado `Total`.
10. Calcule um desconto de 4,5% e apresente o preço com desconto.

---

# ✅ Checklist da Aula 01

- [ ] Criar o banco `aula1`
- [ ] Selecionar o banco
- [ ] Criar a tabela `produto`
- [ ] Inserir os registros
- [ ] Consultar os dados
- [ ] Utilizar `ORDER BY`
- [ ] Utilizar `LIMIT`
- [ ] Realizar cálculos no `SELECT`
- [ ] Utilizar `AS`
- [ ] Calcular desconto
- [ ] Executar o laboratório completo

---

## 📌 Comandos principais

| Comando | Função |
|---|---|
| `SHOW DATABASES` | Lista os bancos |
| `CREATE DATABASE` | Cria um banco |
| `USE` | Seleciona um banco |
| `DROP DATABASE` | Remove um banco |
| `CREATE TABLE` | Cria uma tabela |
| `SHOW TABLES` | Lista as tabelas |
| `DESC` | Exibe a estrutura da tabela |
| `INSERT INTO` | Insere registros |
| `SELECT` | Consulta dados |
| `ORDER BY` | Ordena resultados |
| `LIMIT` | Limita resultados |
| `AS` | Cria alias |

---

## 📚 Fonte da aula

Conteúdo estruturado a partir do material fornecido para a **Aula 1 de Banco de Dados**, mantendo os comandos e exemplos SQL presentes no arquivo original. 

---

**Professor: Ygor Silva**  
**Curso: Banco de Dados com MySQL**  
**Aula 01 — Fundamentos de SQL**
