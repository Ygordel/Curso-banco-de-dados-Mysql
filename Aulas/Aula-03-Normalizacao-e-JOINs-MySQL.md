# 🗄️ Aula 03 — Normalização, Relacionamentos e JOINs no MySQL

**Professor:** Ygor Silva  
**Curso:** Banco de Dados — MySQL  
**Aula:** 03  
**Tema:** Normalização, relacionamentos, chaves estrangeiras e consultas com JOIN

---

## 🎯 Objetivos da aula

Ao final desta aula, o aluno deverá ser capaz de:

- compreender o conceito de **normalização**;
- identificar a finalidade das **Formas Normais**;
- reconhecer campos compostos e multivalorados;
- compreender relacionamentos entre tabelas;
- diferenciar **Chave Primária (PK)** e **Chave Estrangeira (FK)**;
- criar tabelas relacionadas no MySQL;
- utilizar `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL` e `AUTO_INCREMENT`;
- inserir e consultar dados relacionados;
- utilizar `IN`, `NOT IN`, `BETWEEN` e `NOT BETWEEN`;
- realizar consultas envolvendo duas ou mais tabelas;
- utilizar `INNER JOIN`, `LEFT JOIN` e `RIGHT JOIN`.

---

# 1. 📚 Normalização

A **normalização** é um conjunto de regras utilizado no projeto de bancos de dados para organizar entidades, atributos e relacionamentos.

O objetivo é estruturar os dados de maneira adequada, reduzindo problemas relacionados a:

- duplicidade de informações;
- inconsistência dos dados;
- manutenção das tabelas;
- integridade dos registros;
- organização do modelo de dados.

As Formas Normais são aplicadas de maneira progressiva. Uma estrutura que atende à **FN3**, por exemplo, deve atender anteriormente às condições da **FN1** e da **FN2**.

---

# 2. 🔹 Primeira Forma Normal — FN1

A **Primeira Forma Normal (FN1)** determina que os campos devem armazenar valores **atômicos**, ou seja, valores que possam ser tratados individualmente.

### ❌ Exemplo inadequado — campo composto

```text
Endereco = "Rua Rio Branco, 185, Centro, Rio de Janeiro, RJ"
```

Nesse caso, várias informações diferentes estão armazenadas em um único campo.

### ❌ Exemplo inadequado — campo multivalorado

```text
Telefone = "4444-6666 / 3333-1111"
```

Existem dois telefones armazenados no mesmo campo.

### ✅ Estrutura recomendada

Separar as informações:

```text
rua
numero
bairro
cidade
uf
```

E, quando necessário, criar uma tabela própria para informações que podem possuir múltiplas ocorrências.

---

# 3. 🔹 Segunda Forma Normal — FN2

A **Segunda Forma Normal (FN2)** exige que os atributos da tabela dependam diretamente da chave primária.

Na prática, a estrutura deve evitar atributos que dependam apenas de parte de uma chave composta.

A aplicação das formas normais deve ser analisada considerando as chaves e os relacionamentos existentes no modelo.

---

# 4. 🔗 Relacionamentos

Um relacionamento é utilizado quando existe uma associação entre registros de diferentes tabelas.

Exemplo:

```text
FUNCIONÁRIO
     │
     ├────────── possui ──────────► DEPENDENTE
     │
     └────────── possui ──────────► ENDEREÇO
```

No modelo relacional, essa associação normalmente é implementada por meio de uma **Chave Estrangeira (Foreign Key — FK)**.

A FK de uma tabela referencia a PK de outra tabela.

---

# 5. 🧩 Estudo de caso

Uma empresa precisa armazenar informações de seus funcionários e respectivos dependentes.

Também será necessário registrar o endereço de cada funcionário.

### Dados envolvidos

**Funcionário**

- código;
- nome;
- CPF;
- sexo;
- salário.

**Dependente**

- código;
- nome;
- grau de parentesco;
- telefone;
- funcionário responsável.

**Endereço**

- código;
- rua;
- número;
- cidade;
- UF;
- funcionário responsável.

---

# 6. 📐 Modelo de Entidades e Relacionamentos — MER

O modelo conceitual representa as entidades e seus relacionamentos antes da implementação física no banco.

```text
┌──────────────────┐
│   FUNCIONARIO    │
├──────────────────┤
│ PK codFuncionario│
│ nome             │
│ cpf              │
│ sexo             │
│ salario          │
└────────┬─────────┘
         │
         │ 1:N
         │
         ▼
┌──────────────────┐
│    DEPENDENTE    │
├──────────────────┤
│ PK codDependente │
│ nome             │
│ grauParent       │
│ telefone         │
│ FK cod_func      │
└──────────────────┘

┌──────────────────┐
│   FUNCIONARIO    │
└────────┬─────────┘
         │
         │ 1:1
         │
         ▼
┌──────────────────┐
│     ENDERECO     │
├──────────────────┤
│ PK codEndereco   │
│ rua              │
│ numero           │
│ cidade           │
│ uf               │
│ FK codFunc       │
└──────────────────┘
```

### Cardinalidades

```text
Funcionário 1 ───────── N Dependentes

Funcionário 1 ───────── 1 Endereço
```

No estudo de caso, um funcionário pode possuir vários dependentes, enquanto o endereço foi modelado como uma relação de um para um.

---

# 7. 🖼️ Diagramas da aula

Os diagramas utilizados na aula podem ser adicionados ao repositório com os seguintes nomes:

```text
imagens/
├── MER-Aula-03.jpg
└── DER-Aula-03.jpg
```

### Modelo de Entidades e Relacionamentos

![Modelo de Entidades e Relacionamentos](imagens/MER-Aula-03.jpg)

### Diagrama de Entidades e Relacionamentos

![Diagrama de Entidades e Relacionamentos](imagens/DER-Aula-03.jpg)

> Caso as imagens ainda não estejam no repositório, crie a pasta `imagens` e envie os arquivos correspondentes.

---

# 8. 🛠️ Criando o banco de dados

Para iniciar o laboratório:

```sql
DROP DATABASE IF EXISTS aula3;

CREATE DATABASE aula3;

USE aula3;
```

O `DROP DATABASE IF EXISTS` permite reiniciar o laboratório removendo uma base anterior, caso ela exista.

---

# 9. 👨‍💼 Criando a tabela FUNCIONARIO

```sql
CREATE TABLE funcionario (
    codFuncionario INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    cpf CHAR(14) NOT NULL UNIQUE,
    sexo ENUM('f', 'm') NOT NULL,
    salario FLOAT NOT NULL
);
```

### Recursos utilizados

| Recurso | Finalidade |
|---|---|
| `PRIMARY KEY` | Identifica exclusivamente cada registro |
| `AUTO_INCREMENT` | Gera automaticamente o código |
| `NOT NULL` | Impede valores nulos |
| `UNIQUE` | Impede duplicidade no CPF |
| `ENUM` | Limita os valores aceitos |

---

# 10. 👨‍👩‍👧 Criando a tabela DEPENDENTE

```sql
CREATE TABLE dependente (
    codDependente INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    grauParent VARCHAR(20) NOT NULL,
    telefone VARCHAR(15) NOT NULL,
    cod_func INTEGER NOT NULL,
    FOREIGN KEY (cod_func)
        REFERENCES funcionario (codFuncionario)
);
```

### Relação

```text
funcionario.codFuncionario
            │
            │ referência
            ▼
dependente.cod_func
```

A coluna `cod_func` identifica o funcionário ao qual o dependente está relacionado.

---

# 11. 🏠 Criando a tabela ENDERECO

```sql
CREATE TABLE endereco (
    codEndereco INTEGER PRIMARY KEY AUTO_INCREMENT,
    rua VARCHAR(80) NOT NULL,
    numero VARCHAR(10) NOT NULL,
    cidade VARCHAR(80) NOT NULL,
    uf CHAR(2) NOT NULL,
    codFunc INTEGER NOT NULL UNIQUE,
    FOREIGN KEY (codFunc)
        REFERENCES funcionario (codFuncionario)
);
```

O `UNIQUE` em `codFunc` impede que o mesmo funcionário seja associado a mais de um registro de endereço nessa modelagem.

---

# 12. 🔎 Conferindo as tabelas

```sql
SHOW TABLES;
```

Consultar a estrutura:

```sql
DESC funcionario;
DESC dependente;
DESC endereco;
```

---

# 13. 📝 Inserindo funcionários

```sql
INSERT INTO funcionario VALUES
(NULL, 'Mel', '345.534.233-27', 'f', 4500),
(NULL, 'Edu', '111.534.233-27', 'm', 3800),
(NULL, 'Bia', '222.534.233-27', 'f', 6300),
(NULL, 'Ana', '333.534.233-27', 'f', 7600),
(NULL, 'Nat', '444.534.233-27', 'f', 4200),
(NULL, 'Leo', '555.534.233-27', 'm', 3500);
```

Consultar:

```sql
SELECT * FROM funcionario;
```

---

# 14. 🏠 Inserindo endereços

```sql
INSERT INTO endereco VALUES
(NULL, 'Rua a', '123', 'Cidade q', 'RJ', 5),
(NULL, 'Rua s', '346', 'Cidade w', 'MG', 1),
(NULL, 'Rua d', '254', 'Cidade e', 'MG', 4),
(NULL, 'Rua f', '542', 'Cidade r', 'RJ', 3),
(NULL, 'Rua g', '566', 'Cidade t', 'RJ', 6),
(NULL, 'Rua h', '321', 'Cidade Y', 'SP', 2);
```

Consultar:

```sql
SELECT * FROM endereco;
```

---

# 15. 👨‍👩‍👧 Inserindo dependentes

```sql
INSERT INTO dependente VALUES
(NULL, 'Beto', 'Filho', '3333-4444', 4),
(NULL, 'Rafa', 'Filha', '2233-4444', 1),
(NULL, 'Mila', 'Filha', '4433-4444', 4),
(NULL, 'Juca', 'Filho', '5533-4444', 3),
(NULL, 'Duda', 'Filha', '6633-4444', 6),
(NULL, 'Cadu', 'Filho', '7733-4444', 2),
(NULL, 'Hugo', 'Filho', '8833-4444', 1),
(NULL, 'Mara', 'Conjuge', '9933-4444', 2),
(NULL, 'João', 'Conjuge', '3355-4444', 4);
```

Consultar:

```sql
SELECT * FROM dependente;
```

---

# 16. 🔍 Operador IN

O `IN` permite pesquisar vários valores de uma mesma coluna.

### Forma tradicional

```sql
SELECT *
FROM endereco
WHERE uf = 'RJ' OR uf = 'SP';
```

### Utilizando IN

```sql
SELECT *
FROM endereco
WHERE uf IN ('RJ', 'SP');
```

As duas consultas representam a mesma ideia.

---

# 17. 🚫 Operador NOT IN

Para localizar estados diferentes de RJ e SP:

```sql
SELECT *
FROM endereco
WHERE uf NOT IN ('RJ', 'SP');
```

---

# 18. 📊 BETWEEN

O operador `BETWEEN` permite consultar valores dentro de um intervalo.

### Sem BETWEEN

```sql
SELECT *
FROM funcionario
WHERE salario >= 4000
  AND salario <= 7000;
```

### Utilizando BETWEEN

```sql
SELECT *
FROM funcionario
WHERE salario BETWEEN 4000 AND 7000;
```

---

# 19. 🚫 NOT BETWEEN

Para consultar salários fora do intervalo:

```sql
SELECT *
FROM funcionario
WHERE salario NOT BETWEEN 4000 AND 7000;
```

Outra forma seria:

```sql
SELECT *
FROM funcionario
WHERE salario < 4000
   OR salario > 7000;
```

---

# 20. 🔗 Consulta entre duas tabelas

É possível relacionar tabelas utilizando a condição correspondente entre suas chaves.

### Forma tradicional

```sql
SELECT nome, sexo, rua, cidade, uf
FROM funcionario, endereco
WHERE codFuncionario = codFunc;
```

Embora funcione, essa sintaxe é apresentada aqui principalmente para comparação com a utilização de `JOIN`.

---

# 21. ⭐ INNER JOIN

O `INNER JOIN` retorna registros que possuem correspondência entre as tabelas relacionadas.

```sql
SELECT nome, sexo, rua, cidade, uf
FROM funcionario
INNER JOIN endereco
    ON codFuncionario = codFunc;
```

### Somente funcionários do sexo feminino

```sql
SELECT nome, sexo, rua, cidade, uf
FROM funcionario
INNER JOIN endereco
    ON codFuncionario = codFunc
WHERE sexo = 'f';
```

---

# 22. 👨‍👩‍👧 INNER JOIN entre FUNCIONARIO e DEPENDENTE

```sql
SELECT
    funcionario.nome AS NomeFuncionario,
    cpf,
    dependente.nome AS NomeDependente,
    grauParent
FROM funcionario
INNER JOIN dependente
    ON codFuncionario = cod_func;
```

### Filtrando filhos e filhas

```sql
SELECT
    funcionario.nome AS NomeFuncionario,
    cpf,
    dependente.nome AS NomeDependente,
    grauParent
FROM funcionario
INNER JOIN dependente
    ON codFuncionario = cod_func
WHERE grauParent IN ('Filho', 'Filha')
ORDER BY funcionario.nome;
```

---

# 23. ⬅️ LEFT JOIN

O `LEFT JOIN` mantém todos os registros da tabela que está à esquerda da consulta, mesmo que não exista correspondência na tabela da direita.

```sql
SELECT
    funcionario.nome AS NomeFuncionario,
    cpf,
    dependente.nome AS NomeDependente,
    grauParent
FROM funcionario
LEFT JOIN dependente
    ON codFuncionario = cod_func;
```

Esse tipo de consulta é útil quando queremos visualizar **todos os funcionários**, inclusive aqueles que não possuem dependentes cadastrados.

---

# 24. ➡️ RIGHT JOIN

O `RIGHT JOIN` mantém todos os registros da tabela que está à direita.

```sql
SELECT
    funcionario.nome AS NomeFuncionario,
    cpf,
    dependente.nome AS NomeDependente,
    grauParent
FROM dependente
RIGHT JOIN funcionario
    ON codFuncionario = cod_func;
```

A ordem das tabelas influencia o resultado do `LEFT JOIN` e do `RIGHT JOIN`.

---

# 25. 🔎 Comparando os JOINs

| JOIN | Resultado |
|---|---|
| `INNER JOIN` | Somente registros com correspondência |
| `LEFT JOIN` | Todos da tabela da esquerda + correspondências |
| `RIGHT JOIN` | Todos da tabela da direita + correspondências |

Exemplo conceitual:

```text
INNER JOIN
A ∩ B

LEFT JOIN
A + correspondências de B

RIGHT JOIN
B + correspondências de A
```

---

# 26. 🧠 Alias com AS

O `AS` permite criar nomes mais claros para as colunas exibidas.

```sql
SELECT
    funcionario.nome AS NomeFuncionario,
    dependente.nome AS NomeDependente
FROM funcionario
INNER JOIN dependente
    ON codFuncionario = cod_func;
```

Resultado conceitual:

```text
NomeFuncionario | NomeDependente
----------------|---------------
Mel             | Rafa
Edu             | Cadu
Bia             | Beto
...
```

---

# 27. 🧪 Laboratório completo

Execute os comandos abaixo em sequência para reproduzir a prática.

```sql
DROP DATABASE IF EXISTS aula3;

CREATE DATABASE aula3;

USE aula3;

CREATE TABLE funcionario (
    codFuncionario INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    cpf CHAR(14) NOT NULL UNIQUE,
    sexo ENUM('f', 'm') NOT NULL,
    salario FLOAT NOT NULL
);

CREATE TABLE dependente (
    codDependente INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    grauParent VARCHAR(20) NOT NULL,
    telefone VARCHAR(15) NOT NULL,
    cod_func INTEGER NOT NULL,
    FOREIGN KEY (cod_func)
        REFERENCES funcionario (codFuncionario)
);

CREATE TABLE endereco (
    codEndereco INTEGER PRIMARY KEY AUTO_INCREMENT,
    rua VARCHAR(80) NOT NULL,
    numero VARCHAR(10) NOT NULL,
    cidade VARCHAR(80) NOT NULL,
    uf CHAR(2) NOT NULL,
    codFunc INTEGER NOT NULL UNIQUE,
    FOREIGN KEY (codFunc)
        REFERENCES funcionario (codFuncionario)
);

INSERT INTO funcionario VALUES
(NULL, 'Mel', '345.534.233-27', 'f', 4500),
(NULL, 'Edu', '111.534.233-27', 'm', 3800),
(NULL, 'Bia', '222.534.233-27', 'f', 6300),
(NULL, 'Ana', '333.534.233-27', 'f', 7600),
(NULL, 'Nat', '444.534.233-27', 'f', 4200),
(NULL, 'Leo', '555.534.233-27', 'm', 3500);

INSERT INTO endereco VALUES
(NULL, 'Rua a', '123', 'Cidade q', 'RJ', 5),
(NULL, 'Rua s', '346', 'Cidade w', 'MG', 1),
(NULL, 'Rua d', '254', 'Cidade e', 'MG', 4),
(NULL, 'Rua f', '542', 'Cidade r', 'RJ', 3),
(NULL, 'Rua g', '566', 'Cidade t', 'RJ', 6),
(NULL, 'Rua h', '321', 'Cidade Y', 'SP', 2);

INSERT INTO dependente VALUES
(NULL, 'Beto', 'Filho', '3333-4444', 4),
(NULL, 'Rafa', 'Filha', '2233-4444', 1),
(NULL, 'Mila', 'Filha', '4433-4444', 4),
(NULL, 'Juca', 'Filho', '5533-4444', 3),
(NULL, 'Duda', 'Filha', '6633-4444', 6),
(NULL, 'Cadu', 'Filho', '7733-4444', 2),
(NULL, 'Hugo', 'Filho', '8833-4444', 1),
(NULL, 'Mara', 'Conjuge', '9933-4444', 2),
(NULL, 'João', 'Conjuge', '3355-4444', 4);

SHOW TABLES;

SELECT * FROM funcionario;

SELECT * FROM endereco;

SELECT * FROM dependente;

SELECT *
FROM endereco
WHERE uf IN ('RJ', 'SP');

SELECT *
FROM endereco
WHERE uf NOT IN ('RJ', 'SP');

SELECT *
FROM funcionario
WHERE salario BETWEEN 4000 AND 7000;

SELECT *
FROM funcionario
WHERE salario NOT BETWEEN 4000 AND 7000;

SELECT
    funcionario.nome AS NomeFuncionario,
    cpf,
    dependente.nome AS NomeDependente,
    grauParent
FROM funcionario
INNER JOIN dependente
    ON codFuncionario = cod_func;

SELECT
    funcionario.nome AS NomeFuncionario,
    cpf,
    dependente.nome AS NomeDependente,
    grauParent
FROM funcionario
LEFT JOIN dependente
    ON codFuncionario = cod_func;
```

---

# 28. 📝 Atividade prática

## Desafio 01 — Consultas com IN

Liste os endereços dos funcionários que estão nos estados:

```text
RJ
SP
```

Utilize `IN`.

---

## Desafio 02 — Consulta com NOT IN

Liste os endereços que não pertencem aos estados:

```text
RJ
SP
```

Utilize `NOT IN`.

---

## Desafio 03 — Faixa salarial

Liste os funcionários que recebem entre:

```text
R$ 4.000
e
R$ 7.000
```

Utilize `BETWEEN`.

---

## Desafio 04 — Funcionários e endereços

Crie uma consulta utilizando `INNER JOIN` para mostrar:

```text
Nome
Sexo
Rua
Cidade
UF
```

---

## Desafio 05 — Funcionários e dependentes

Utilize `INNER JOIN` para apresentar:

```text
Nome do funcionário
CPF
Nome do dependente
Grau de parentesco
```

---

## Desafio 06 — LEFT JOIN

Utilize `LEFT JOIN` para apresentar todos os funcionários, mesmo aqueles que não possuem dependentes cadastrados.

---

# 29. ✅ Checklist da aula

- [ ] Entendi o conceito de normalização.
- [ ] Compreendi a FN1.
- [ ] Compreendi a FN2.
- [ ] Identifiquei campos compostos.
- [ ] Identifiquei campos multivalorados.
- [ ] Entendi PK e FK.
- [ ] Criei tabelas relacionadas.
- [ ] Utilizei `AUTO_INCREMENT`.
- [ ] Utilizei `UNIQUE`.
- [ ] Utilizei `NOT NULL`.
- [ ] Utilizei `ENUM`.
- [ ] Utilizei `IN`.
- [ ] Utilizei `NOT IN`.
- [ ] Utilizei `BETWEEN`.
- [ ] Utilizei `NOT BETWEEN`.
- [ ] Executei um `INNER JOIN`.
- [ ] Executei um `LEFT JOIN`.
- [ ] Executei um `RIGHT JOIN`.
- [ ] Utilizei alias com `AS`.

---

# 📁 Organização sugerida do repositório

```text
curso-banco-de-dados-mysql/
│
├── Aula-01/
│   └── Aula-01-Fundamentos-MySQL.md
│
├── Aula-02/
│   └── Aula-02-Modelagem-de-Dados-MySQL.md
│
├── Aula-03/
│   ├── Aula-03-Normalizacao-e-JOINs-MySQL.md
│   └── imagens/
│       ├── MER-Aula-03.jpg
│       └── DER-Aula-03.jpg
│
├── ferramentas/
│
└── README.md
```

---

# 📌 Resumo

Nesta aula, o banco de dados passou do modelo conceitual para uma estrutura relacional implementada no MySQL.

Os principais conceitos trabalhados foram:

```text
Normalização
      ↓
Entidades
      ↓
Chaves Primárias
      ↓
Chaves Estrangeiras
      ↓
Relacionamentos
      ↓
Tabelas relacionadas
      ↓
Consultas
      ↓
JOINs
```

A prática utiliza o cenário **Funcionário, Dependente e Endereço** para demonstrar como as entidades são separadas, relacionadas e posteriormente consultadas utilizando SQL.

---

**Professor: Ygor Silva**  
**Curso: Banco de Dados — MySQL**  
**Aula 03 — Normalização, Relacionamentos e JOINs**
