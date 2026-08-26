# 🏁 Etapa Conclusiva — Curso de Banco de Dados com MySQL

**Professor:** Ygor Silva  
**Curso:** Banco de Dados — MySQL  
**Etapa:** Conclusão

---

## 🎯 Objetivo

Esta etapa representa o encerramento do percurso inicial do curso de Banco de Dados com foco em MySQL.

Ao longo das aulas, foram trabalhados conceitos fundamentais de bancos de dados relacionais, modelagem, criação de estruturas, relacionamentos e manipulação de informações utilizando SQL.

A proposta agora é reunir esses conhecimentos em uma atividade prática final.

---

# 📚 1. Conhecimentos desenvolvidos

Durante o curso foram estudados conceitos como:

- Banco de Dados;
- Sistema Gerenciador de Banco de Dados;
- MySQL;
- tabelas;
- registros;
- campos;
- chaves primárias;
- chaves estrangeiras;
- relacionamentos;
- cardinalidade;
- modelo conceitual;
- Modelo Entidade-Relacionamento (MER);
- Diagrama Entidade-Relacionamento (DER);
- normalização;
- restrições de integridade;
- `PRIMARY KEY`;
- `FOREIGN KEY`;
- `UNIQUE`;
- `CHECK`;
- `NOT NULL`;
- `CREATE DATABASE`;
- `CREATE TABLE`;
- `INSERT`;
- `UPDATE`;
- `SELECT`;
- `WHERE`;
- `ORDER BY`;
- `GROUP BY`;
- `HAVING`;
- `JOIN`;
- funções de agregação;
- cálculo de médias;
- organização e consulta de dados.

---

# 🧩 2. Projeto final

A atividade conclusiva consiste em desenvolver um pequeno banco de dados utilizando os conhecimentos apresentados durante o curso.

O projeto deverá representar uma situação real.

### Sugestões de temas

- Sistema acadêmico;
- Biblioteca;
- Loja;
- Controle de funcionários;
- Clínica;
- Restaurante;
- Estoque;
- Oficina;
- Empresa;
- Sistema de vendas.

O aluno poderá escolher outro tema, desde que utilize um modelo relacional coerente.

---

# 🏗️ 3. Requisitos mínimos

O projeto deverá possuir:

### Banco de dados

```sql
CREATE DATABASE nome_do_banco;
```

### Tabelas

Criar pelo menos **4 tabelas relacionadas**.

### Chaves

Utilizar:

```text
PRIMARY KEY
FOREIGN KEY
```

### Restrições

Utilizar, quando fizer sentido:

```text
NOT NULL
UNIQUE
CHECK
```

### Relacionamentos

O projeto deverá apresentar relacionamentos entre as tabelas.

---

# 📊 4. Dados para teste

Cada tabela deverá possuir dados suficientes para permitir a realização das consultas.

Como referência:

```text
Tabela principal: pelo menos 8 registros
Tabelas relacionadas: pelo menos 4 registros
```

O objetivo é permitir que os relacionamentos e consultas sejam demonstrados de forma prática.

---

# 🔎 5. Consultas obrigatórias

O projeto deverá apresentar consultas utilizando:

### Consulta simples

```sql
SELECT * FROM tabela;
```

### Filtro

```sql
SELECT *
FROM tabela
WHERE condição;
```

### Ordenação

```sql
SELECT *
FROM tabela
ORDER BY campo;
```

### Relacionamento

```sql
SELECT ...
FROM tabela1
INNER JOIN tabela2
    ON tabela1.id = tabela2.id;
```

### Agrupamento

```sql
SELECT campo, COUNT(*)
FROM tabela
GROUP BY campo;
```

### Função de agregação

Utilizar pelo menos algumas das funções:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

# 🧮 6. Consulta com cálculo

O projeto deverá possuir pelo menos uma consulta que realize algum cálculo.

Exemplo:

```sql
SELECT
    nome,
    ROUND((nota1 + nota2 + nota3) / 3, 2) AS media
FROM notas;
```

Também poderá ser utilizado:

```sql
CASE
    WHEN condição THEN 'Resultado 1'
    ELSE 'Resultado 2'
END
```

---

# 🔗 7. Demonstração dos relacionamentos

O aluno deverá conseguir explicar:

```text
Qual é a chave primária?
Qual é a chave estrangeira?
Qual tabela depende de outra?
Qual é o relacionamento?
Qual é a cardinalidade?
```

Exemplo:

```text
CLIENTE
   │
   └──── PEDIDO
```

Um cliente pode possuir vários pedidos.

---

# 📝 8. Entrega do projeto

A entrega deverá apresentar:

### 01 — Modelo

DER ou MER do banco desenvolvido.

### 02 — Script SQL

Arquivo contendo:

```text
CREATE DATABASE
CREATE TABLE
INSERT
UPDATE
SELECT
```

e demais comandos utilizados no projeto.

### 03 — Consultas

Arquivo contendo as principais consultas desenvolvidas.

### 04 — Documentação

Arquivo `README.md` explicando:

- objetivo do projeto;
- problema solucionado;
- tabelas;
- relacionamentos;
- principais consultas;
- tecnologias utilizadas;
- conclusão.

---

# 💻 9. Estrutura sugerida dos arquivos

Uma estrutura simples para apresentação do projeto:

```text
projeto-final/
│
├── README.md
│
├── banco.sql
│
├── consultas.sql
│
└── imagens/
    └── der.png
```

---

# 🧪 10. Checklist técnico

Antes de finalizar o projeto, verifique:

- [ ] Banco criado.
- [ ] Tabelas criadas.
- [ ] Chaves primárias configuradas.
- [ ] Chaves estrangeiras configuradas.
- [ ] Restrições de integridade configuradas.
- [ ] Relacionamentos funcionando.
- [ ] Dados inseridos.
- [ ] Consultas executadas.
- [ ] `JOIN` utilizado.
- [ ] `GROUP BY` utilizado.
- [ ] Funções de agregação utilizadas.
- [ ] Cálculos realizados.
- [ ] DER/MER elaborado.
- [ ] Script SQL organizado.
- [ ] README criado.
- [ ] Projeto testado no MySQL.

---

# 🎓 11. Resultado esperado

Ao concluir esta etapa, o aluno deverá ser capaz de criar uma estrutura básica de banco de dados relacional e utilizar SQL para:

```text
CRIAR
   ↓
ESTRUTURAR
   ↓
RELACIONAR
   ↓
CADASTRAR
   ↓
ALTERAR
   ↓
CONSULTAR
   ↓
ANALISAR
```

O objetivo principal não é apenas memorizar comandos SQL, mas compreender como os dados são organizados e relacionados dentro de um banco de dados.

---

# 🚀 12. Próximos passos

Depois da conclusão do conteúdo introdutório, o estudo poderá avançar para temas mais específicos de MySQL, como:

- consultas SQL avançadas;
- subconsultas;
- `VIEW`;
- índices;
- procedimentos armazenados;
- funções;
- `TRIGGER`;
- transações;
- controle de usuários;
- privilégios;
- backup e restauração;
- otimização de consultas;
- administração do MySQL;
- segurança de banco de dados;
- integração entre aplicações e MySQL.

---

# 🏆 Conclusão

O curso apresentou uma base prática para compreender o funcionamento de bancos de dados relacionais e aplicar SQL no MySQL.

A partir dos conceitos de modelagem e relacionamento, foi possível evoluir para a criação das tabelas, definição das restrições, inserção de informações, atualização dos registros e elaboração de consultas.

A etapa final consolida esses conhecimentos por meio de um projeto prático, permitindo que o aluno demonstre sua capacidade de transformar um problema do mundo real em uma estrutura organizada de dados.

## ✅ Curso concluído

**Banco de Dados — MySQL**

**Professor: Ygor Silva**

> Conhecer SQL é aprender a transformar dados em informação organizada, consultável e útil para a tomada de decisões.
