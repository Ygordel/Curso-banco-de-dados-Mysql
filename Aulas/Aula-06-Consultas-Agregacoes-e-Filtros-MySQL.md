# 🗄️ Aula 06 — Consultas, Agregações e Filtros no MySQL

**Curso:** Banco de Dados — MySQL  
**Aula:** 06  
**Tema:** Alteração de tabelas, relacionamentos, JOIN, funções de agregação, DISTINCT, GROUP BY, WHERE e HAVING

---

## 🎯 Objetivos da aula

Nesta aula vamos continuar o estudo do banco de dados criado anteriormente e praticar:

- alteração da estrutura de tabelas;
- inclusão de colunas;
- criação de `FOREIGN KEY`;
- criação de restrições `UNIQUE`;
- inserção de registros;
- consultas com `INNER JOIN`;
- consultas com `LEFT JOIN`;
- identificação de registros sem relacionamento;
- funções de agregação;
- `COUNT()`;
- `SUM()`;
- `AVG()`;
- `MIN()`;
- `MAX()`;
- `DISTINCT`;
- `GROUP BY`;
- agrupamento por mais de uma coluna;
- diferença entre `WHERE` e `HAVING`.

---

# 1. 📚 Continuação do modelo de funcionários

Na aula anterior foi construído um modelo envolvendo:

- funcionários;
- cargos;
- cônjuges;
- projetos;
- alocação de funcionários em projetos.

Nesta etapa vamos complementar o relacionamento entre `funcionario` e `cargo`, inserir dados e realizar consultas sobre essas informações.

---

# 2. 🏗️ Estrutura inicial

Caso seja necessário recriar o banco para acompanhar a aula desde o início:

```sql
DROP DATABASE IF EXISTS aula6;

CREATE DATABASE aula6;

USE aula6;
```

---

# 3. 👷 Criando a tabela `funcionario`

```sql
CREATE TABLE funcionario (
    codFuncionario INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL,
    sexo ENUM('f', 'm') NOT NULL,
    estadoCivil VARCHAR(30) NOT NULL,
    cpf CHAR(14) NOT NULL
);
```

A tabela inicialmente possui os dados básicos do funcionário.

---

# 4. 💼 Criando a tabela `cargo`

```sql
CREATE TABLE cargo (
    codCargo INTEGER PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(70) NOT NULL,
    salario DECIMAL(8,2) NOT NULL
);
```

O salário fica relacionado ao cargo.

Dessa forma, funcionários que ocupam o mesmo cargo podem utilizar a mesma informação de salário armazenada na tabela `cargo`.

---

# 5. 💍 Criando a tabela `conjuge`

```sql
CREATE TABLE conjuge (
    codConjuge INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL,
    telefone VARCHAR(20) NOT NULL,
    codFunc INTEGER NOT NULL,

    FOREIGN KEY (codFunc)
        REFERENCES funcionario(codFuncionario)
);
```

O campo `codFunc` estabelece o relacionamento com `funcionario`.

---

# 6. 📋 Criando a tabela `projeto`

```sql
CREATE TABLE projeto (
    codProjeto INTEGER AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    dataInicio DATE NOT NULL,
    tempPrev INTEGER NOT NULL,
    porcent FLOAT NOT NULL,

    PRIMARY KEY (codProjeto)
);
```

---

# 7. 🔗 Criando a tabela associativa `alocado`

A relação entre funcionário e projeto é do tipo **muitos para muitos**.

Por isso utilizamos a tabela `alocado`.

```sql
CREATE TABLE alocado (
    cod_func INTEGER NOT NULL,
    cod_proj INTEGER NOT NULL,

    PRIMARY KEY (cod_func, cod_proj),

    FOREIGN KEY (cod_func)
        REFERENCES funcionario(codFuncionario),

    FOREIGN KEY (cod_proj)
        REFERENCES projeto(codProjeto)
);
```

A chave primária é composta por:

```text
cod_func + cod_proj
```

---

# 8. 🔎 Visualizando tabelas e estruturas

Para visualizar todas as tabelas:

```sql
SHOW TABLES;
```

Para visualizar a estrutura:

```sql
DESC cargo;
```

```sql
DESC funcionario;
```

```sql
DESC conjuge;
```

```sql
DESC projeto;
```

```sql
DESC alocado;
```

---

# 9. 🛠️ Alterando uma tabela com ALTER TABLE

A estrutura de `funcionario` será complementada com o código do cargo.

```sql
ALTER TABLE funcionario
ADD COLUMN cod_cargo INT NOT NULL;
```

Agora a tabela `funcionario` passa a possuir uma referência para `cargo`.

---

# 10. 🔐 Criando uma FOREIGN KEY posteriormente

A restrição de chave estrangeira pode ser adicionada depois da criação da tabela.

```sql
ALTER TABLE funcionario
ADD CONSTRAINT fk_cargo
FOREIGN KEY (cod_cargo)
REFERENCES cargo(codCargo);
```

O nome da restrição utilizado no exemplo é:

```text
fk_cargo
```

---

# 11. 🔒 Criando uma restrição UNIQUE

O CPF deve ser único para cada funcionário.

```sql
ALTER TABLE funcionario
ADD CONSTRAINT unq_cpf
UNIQUE(cpf);
```

Assim, o banco impede o cadastro de dois funcionários com o mesmo CPF.

---

# 12. 🔒 Garantindo um único cônjuge por funcionário

Como o relacionamento apresentado no modelo permite apenas um cônjuge associado ao funcionário, podemos garantir essa regra com `UNIQUE`.

```sql
ALTER TABLE conjuge
ADD CONSTRAINT unq_codFunc
UNIQUE(codFunc);
```

---

# 13. 🔎 Conferindo as alterações

```sql
DESC funcionario;
```

```sql
DESC conjuge;
```

---

# 14. 📥 Inserindo cargos

Vamos cadastrar alguns cargos:

```sql
INSERT INTO cargo(titulo, salario)
VALUES
    ('Estagio', 1400),
    ('Junior', 3350),
    ('Pleno', 6100),
    ('Senior', 9800),
    ('Gerente', 13000);
```

Consultar:

```sql
SELECT * FROM cargo;
```

---

# 15. 👷 Inserindo funcionários

```sql
INSERT INTO funcionario
VALUES
    (NULL, 'Bia', 'f', 'Solteiro', '123.312.312-21', 4),
    (NULL, 'Leo', 'm', 'Casado', '132.312.312-17', 3),
    (NULL, 'Ana', 'f', 'Casado', '045.312.312-05', 1),
    (NULL, 'Mel', 'f', 'Solteiro', '221.312.312-15', 1),
    (NULL, 'Edu', 'm', 'Solteiro', '034.312.312-38', 2),
    (NULL, 'Rui', 'm', 'Solteiro', '441.312.312-06', 1),
    (NULL, 'Nat', 'f', 'Solteiro', '934.312.312-17', 2);
```

Consultar:

```sql
SELECT * FROM funcionario;
```

---

# 16. 💍 Inserindo cônjuges

```sql
INSERT INTO conjuge
VALUES
    (NULL, 'Beto', '2333-4222', 3),
    (NULL, 'Duda', '5333-4221', 2);
```

Consultar:

```sql
SELECT * FROM conjuge;
```

---

# 17. 📊 Inserindo projetos

```sql
INSERT INTO projeto
    (codProjeto, nome, valor, dataInicio, tempPrev, porcent)
VALUES
    (NULL, 'E-Commerce', 230000, '2022-04-14', 170, 1),
    (NULL, 'Gestão Hospitalar', 420000, '2022-05-10', 225, 0.5);
```

Consultar:

```sql
SELECT * FROM projeto;
```

---

# 18. 🔗 Inserindo alocações

```sql
INSERT INTO alocado(cod_proj, cod_func)
VALUES
    (2, 5),
    (1, 3),
    (1, 2),
    (2, 6),
    (2, 7),
    (1, 1),
    (2, 4);
```

Consultar:

```sql
SELECT * FROM alocado;
```

---

# 19. 🔎 INNER JOIN

O `INNER JOIN` retorna registros que possuem correspondência entre as tabelas relacionadas.

Exemplo:

```sql
SELECT
    nome,
    titulo,
    salario
FROM cargo
INNER JOIN funcionario
    ON codCargo = cod_cargo;
```

Resultado conceitual:

```text
Funcionário | Cargo   | Salário
------------|---------|--------
Bia         | Senior  | 9800
Leo         | Pleno   | 6100
Ana         | Estagio | 1400
...
```

---

# 20. 🔎 LEFT JOIN

O `LEFT JOIN` mantém os registros da tabela localizada à esquerda, mesmo quando não existe correspondência na tabela da direita.

```sql
SELECT
    nome,
    titulo,
    salario
FROM cargo
LEFT JOIN funcionario
    ON codCargo = cod_cargo;
```

Esse tipo de consulta é útil quando queremos identificar também cargos que ainda não possuem funcionários associados.

---

# 21. 🔎 Encontrando cargos sem funcionários

Podemos combinar `LEFT JOIN` com `IS NULL`.

```sql
SELECT
    titulo,
    COUNT(nome) AS QTD
FROM cargo
LEFT JOIN funcionario
    ON codCargo = cod_cargo
WHERE cod_cargo IS NULL
GROUP BY titulo;
```

Outra forma simples de visualizar os cargos sem funcionário:

```sql
SELECT
    cargo.titulo
FROM cargo
LEFT JOIN funcionario
    ON cargo.codCargo = funcionario.cod_cargo
WHERE funcionario.cod_cargo IS NULL;
```

---

# 22. 👷 Consultando funcionários e projetos

Para obter os funcionários e os projetos em que estão alocados:

```sql
SELECT
    f.nome AS Funcionario,
    f.cpf,
    p.nome AS Projeto,
    p.valor
FROM funcionario AS f
INNER JOIN alocado AS a
    ON f.codFuncionario = a.cod_func
INNER JOIN projeto AS p
    ON p.codProjeto = a.cod_proj;
```

---

# 23. 👩 Consultando somente funcionárias

Podemos utilizar `WHERE` para filtrar os resultados.

```sql
SELECT
    f.nome AS Funcionario,
    f.cpf,
    p.nome AS Projeto,
    p.valor
FROM funcionario AS f
INNER JOIN alocado AS a
    ON f.codFuncionario = a.cod_func
INNER JOIN projeto AS p
    ON p.codProjeto = a.cod_proj
WHERE f.sexo = 'f'
ORDER BY f.nome;
```

---

# 24. 📊 Funções de agregação

As funções de agregação permitem realizar cálculos sobre conjuntos de registros.

Principais funções trabalhadas:

| Função | Finalidade |
|---|---|
| `COUNT()` | Conta registros |
| `SUM()` | Soma valores |
| `AVG()` | Calcula média |
| `MIN()` | Obtém menor valor |
| `MAX()` | Obtém maior valor |

---

# 25. 🔢 COUNT()

Para contar funcionários:

```sql
SELECT COUNT(nome)
FROM funcionario;
```

Para contar projetos:

```sql
SELECT COUNT(codProjeto) AS QTD
FROM projeto;
```

Também podemos utilizar:

```sql
SELECT COUNT(*) AS Quantidade
FROM funcionario;
```

---

# 26. ➕ SUM()

Para calcular o valor total dos projetos:

```sql
SELECT
    SUM(valor) AS Soma
FROM projeto;
```

---

# 27. 📈 AVG()

Para calcular o valor médio dos projetos:

```sql
SELECT
    AVG(valor) AS Media
FROM projeto;
```

---

# 28. ⬇️ MIN()

Para identificar o menor salário:

```sql
SELECT
    MIN(salario) AS Menor
FROM cargo;
```

---

# 29. ⬆️ MAX()

Para identificar o maior salário:

```sql
SELECT
    MAX(salario) AS Maior
FROM cargo;
```

---

# 30. 📊 Utilizando várias funções de agregação

Podemos executar vários cálculos na mesma consulta:

```sql
SELECT
    COUNT(codProjeto) AS QTD,
    SUM(valor) AS Soma,
    AVG(valor) AS Media
FROM projeto;
```

---

# 31. 🎯 DISTINCT

O `DISTINCT` elimina valores duplicados do resultado.

Para descobrir quais gêneros existem:

```sql
SELECT DISTINCT sexo
FROM funcionario;
```

Para descobrir os estados civis existentes:

```sql
SELECT DISTINCT estadoCivil
FROM funcionario;
```

---

# 32. 🔎 DISTINCT com mais de uma coluna

Também podemos combinar várias colunas:

```sql
SELECT DISTINCT
    sexo,
    estadoCivil
FROM funcionario;
```

Podemos organizar o resultado:

```sql
SELECT DISTINCT
    sexo,
    estadoCivil
FROM funcionario
ORDER BY sexo;
```

---

# 33. 📦 GROUP BY

O `GROUP BY` agrupa registros que possuem valores iguais em uma ou mais colunas.

Exemplo: quantidade de funcionários por sexo.

```sql
SELECT
    sexo,
    COUNT(nome) AS QTD
FROM funcionario
GROUP BY sexo;
```

Resultado conceitual:

```text
sexo | QTD
-----|----
f    | ...
m    | ...
```

---

# 34. 👨‍👩‍👧‍👦 Agrupando por estado civil

```sql
SELECT
    estadoCivil,
    COUNT(nome) AS QTD
FROM funcionario
GROUP BY estadoCivil;
```

---

# 35. 💼 Quantidade de funcionários por cargo

```sql
SELECT
    titulo,
    COUNT(nome) AS QTD
FROM cargo
INNER JOIN funcionario
    ON codCargo = cod_cargo
GROUP BY titulo;
```

---

# 36. 💰 Estatísticas salariais por sexo

Podemos utilizar várias funções de agregação no mesmo agrupamento:

```sql
SELECT
    sexo,
    COUNT(nome) AS QTD,
    SUM(salario) AS SomaSalario,
    ROUND(AVG(salario), 2) AS MediaSalario
FROM cargo
INNER JOIN funcionario
    ON codCargo = cod_cargo
GROUP BY sexo;
```

Aqui são obtidos:

- quantidade de funcionários;
- soma dos salários;
- média salarial.

---

# 37. 🧩 GROUP BY com mais de um campo

Também podemos agrupar por duas ou mais colunas.

```sql
SELECT
    sexo,
    estadoCivil,
    COUNT(nome) AS QTD
FROM funcionario
GROUP BY sexo, estadoCivil;
```

Organizando o resultado:

```sql
SELECT
    sexo,
    estadoCivil,
    COUNT(nome) AS QTD
FROM funcionario
GROUP BY sexo, estadoCivil
ORDER BY sexo, estadoCivil;
```

---

# 38. 🔍 WHERE com GROUP BY

O `WHERE` é utilizado para filtrar os registros **antes** do agrupamento.

Exemplo:

```sql
SELECT
    sexo,
    COUNT(nome) AS QTD
FROM funcionario
WHERE sexo = 'f'
GROUP BY sexo;
```

Neste caso, somente os registros de funcionários do sexo feminino participam do agrupamento.

---

# 39. 🎯 HAVING

O `HAVING` é utilizado para filtrar os grupos produzidos pelo `GROUP BY`.

Exemplo:

```sql
SELECT
    sexo,
    COUNT(nome) AS QTD
FROM funcionario
GROUP BY sexo
HAVING COUNT(nome) > 3;
```

A diferença principal é:

```text
WHERE
↓
filtra registros

GROUP BY
↓
forma grupos

HAVING
↓
filtra grupos
```

---

# 40. 🔎 WHERE + GROUP BY + HAVING

Podemos utilizar os três recursos na mesma consulta:

```sql
SELECT
    titulo,
    COUNT(nome) AS QTD
FROM cargo
INNER JOIN funcionario
    ON codCargo = cod_cargo
WHERE titulo IN ('Junior', 'Pleno')
GROUP BY titulo
HAVING COUNT(nome) > 1;
```

A consulta:

1. seleciona cargos `Junior` e `Pleno`;
2. relaciona cargo e funcionário;
3. agrupa os funcionários por cargo;
4. mantém somente os grupos que possuem mais de um funcionário.

---

# 41. 🧠 WHERE x HAVING

| Recurso | Utilização |
|---|---|
| `WHERE` | Filtra registros antes do agrupamento |
| `GROUP BY` | Cria grupos |
| `HAVING` | Filtra os grupos |
| `ORDER BY` | Ordena o resultado |

Exemplo:

```sql
SELECT
    sexo,
    COUNT(*) AS quantidade
FROM funcionario
WHERE estadoCivil = 'Solteiro'
GROUP BY sexo
HAVING COUNT(*) >= 2
ORDER BY sexo;
```

---

# 42. 🗺️ Fluxo de processamento da consulta

Uma forma simples de memorizar:

```text
FROM / JOIN
     ↓
WHERE
     ↓
GROUP BY
     ↓
HAVING
     ↓
SELECT
     ↓
ORDER BY
```

---

# 43. 🧪 Laboratório completo

Execute as consultas abaixo para revisar os principais conceitos da aula.

### Quantidade de funcionários

```sql
SELECT COUNT(*) AS quantidade
FROM funcionario;
```

### Menor salário

```sql
SELECT MIN(salario) AS menor
FROM cargo;
```

### Maior salário

```sql
SELECT MAX(salario) AS maior
FROM cargo;
```

### Média dos salários

```sql
SELECT ROUND(AVG(salario), 2) AS media
FROM cargo;
```

### Valor total dos projetos

```sql
SELECT SUM(valor) AS total
FROM projeto;
```

### Funcionários por sexo

```sql
SELECT
    sexo,
    COUNT(*) AS quantidade
FROM funcionario
GROUP BY sexo;
```

### Funcionários por estado civil

```sql
SELECT
    estadoCivil,
    COUNT(*) AS quantidade
FROM funcionario
GROUP BY estadoCivil;
```

### Funcionários e seus cargos

```sql
SELECT
    f.nome,
    c.titulo,
    c.salario
FROM funcionario f
INNER JOIN cargo c
    ON f.cod_cargo = c.codCargo;
```

### Funcionários e seus projetos

```sql
SELECT
    f.nome AS funcionario,
    p.nome AS projeto
FROM funcionario f
INNER JOIN alocado a
    ON f.codFuncionario = a.cod_func
INNER JOIN projeto p
    ON p.codProjeto = a.cod_proj;
```

---

# 44. 📝 Exercícios

## Exercício 01

Liste todos os funcionários e seus respectivos cargos.

---

## Exercício 02

Mostre somente os funcionários do sexo feminino.

---

## Exercício 03

Apresente a quantidade de funcionários cadastrados.

---

## Exercício 04

Apresente o maior e o menor salário cadastrado.

---

## Exercício 05

Calcule a média dos salários dos cargos.

---

## Exercício 06

Calcule o valor total dos projetos.

---

## Exercício 07

Mostre a quantidade de funcionários por sexo.

---

## Exercício 08

Mostre a quantidade de funcionários por estado civil.

---

## Exercício 09

Mostre a quantidade de funcionários por cargo.

---

## Exercício 10

Liste os funcionários e os projetos nos quais estão alocados.

---

## Exercício 11

Utilize `DISTINCT` para apresentar os estados civis existentes.

---

## Exercício 12

Utilize `GROUP BY` para apresentar a quantidade de funcionários por sexo.

---

## Exercício 13

Utilize `HAVING` para apresentar somente os grupos que possuem mais de três funcionários.

---

## Exercício 14

Apresente os cargos `Junior` e `Pleno` que possuem mais de um funcionário.

---

## Exercício 15

Explique com suas palavras a diferença entre:

```text
WHERE
```

e

```text
HAVING
```

---

# 45. ✅ Checklist da Aula 06

- [ ] Entender `ALTER TABLE`.
- [ ] Adicionar uma coluna.
- [ ] Criar `FOREIGN KEY`.
- [ ] Criar `UNIQUE`.
- [ ] Inserir registros.
- [ ] Utilizar `INNER JOIN`.
- [ ] Utilizar `LEFT JOIN`.
- [ ] Identificar registros sem relacionamento.
- [ ] Utilizar `COUNT()`.
- [ ] Utilizar `SUM()`.
- [ ] Utilizar `AVG()`.
- [ ] Utilizar `MIN()`.
- [ ] Utilizar `MAX()`.
- [ ] Utilizar `DISTINCT`.
- [ ] Utilizar `GROUP BY`.
- [ ] Agrupar por duas colunas.
- [ ] Utilizar `WHERE`.
- [ ] Utilizar `HAVING`.
- [ ] Combinar `WHERE`, `GROUP BY` e `HAVING`.

---

# 📌 Resumo da Aula

Nesta aula avançamos da modelagem para a exploração dos dados armazenados no banco.

Aprendemos a modificar estruturas existentes com `ALTER TABLE`, criar restrições como `FOREIGN KEY` e `UNIQUE` e inserir dados nas tabelas.

Na parte de consultas, trabalhamos com `INNER JOIN` e `LEFT JOIN` para combinar informações de diferentes tabelas.

Também estudamos as funções de agregação:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

Por fim, utilizamos:

```text
DISTINCT
GROUP BY
WHERE
HAVING
ORDER BY
```

Esses recursos formam uma base importante para consultas SQL mais elaboradas e para a análise dos dados armazenados no MySQL.

---

# 🚀 Próxima etapa

Na próxima aula, continue praticando consultas, funções e recursos de programação do banco de dados a partir do modelo construído nesta etapa.

