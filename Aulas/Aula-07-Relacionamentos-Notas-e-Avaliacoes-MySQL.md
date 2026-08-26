# 🗄️ Aula 07 — Relacionamentos, Notas e Avaliações no MySQL

**Curso:** Banco de Dados — MySQL  
**Aula:** 07  
**Tema:** Relacionamentos, chaves estrangeiras, restrições `CHECK` e `UNIQUE`, lançamento de notas e consultas

---

## 🎯 Objetivos da aula

Nesta aula vamos desenvolver um banco de dados para uma instituição de ensino.

Ao final da prática, você deverá compreender como:

- cadastrar alunos;
- cadastrar responsáveis;
- cadastrar professores;
- cadastrar disciplinas;
- relacionar disciplinas e professores;
- registrar as notas dos alunos;
- utilizar `PRIMARY KEY`;
- utilizar `FOREIGN KEY`;
- utilizar `UNIQUE`;
- utilizar `CHECK`;
- trabalhar com chave primária composta;
- alterar registros com `UPDATE`;
- consultar os dados cadastrados;
- organizar um modelo de controle acadêmico.

---

# 1. 🎓 Estudo de caso

Uma instituição de ensino precisa controlar as avaliações dos alunos nas disciplinas que eles cursam.

O sistema deverá armazenar:

### Alunos

Cada aluno possui:

- matrícula;
- nome;
- e-mail;
- sexo;
- data de nascimento.

Para alguns alunos também será necessário cadastrar um responsável, contendo:

- nome;
- CPF;
- matrícula do aluno.

### Professores

Cada professor possui:

- código;
- nome;
- titulação.

Um professor pode ministrar várias disciplinas.

Uma disciplina, porém, será vinculada a apenas um professor.

Também é possível existir:

- professor sem disciplina;
- disciplina sem professor.

### Disciplinas

Cada disciplina possui:

- código;
- nome;
- carga horária;
- professor responsável.

### Notas

Em cada ano letivo, um aluno pode cursar uma disciplina e receber três notas.

Essas notas serão utilizadas posteriormente para calcular a média e verificar a situação acadêmica.

---

# 2. 🧩 Modelo lógico

O banco será composto pelas seguintes tabelas:

```text
ALUNO
  │
  └── RESPONSAVEL

PROFESSOR
  │
  └── DISCIPLINA

ALUNO ───── NOTAS ───── DISCIPLINA
```

A tabela `notas` funciona como uma tabela associativa entre alunos e disciplinas.

---

# 3. 🏗️ Criando o banco de dados

Caso seja necessário iniciar novamente:

```sql
DROP DATABASE IF EXISTS aula7;

CREATE DATABASE aula7;

USE aula7;
```

> ⚠️ O comando `DROP DATABASE` apaga o banco existente. Utilize somente quando tiver certeza de que os dados podem ser removidos.

---

# 4. 👨‍🎓 Criando a tabela aluno

```sql
CREATE TABLE aluno (
    matricula INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    email VARCHAR(60) NOT NULL,
    sexo CHAR(1) NOT NULL,
    dataNasc DATE NOT NULL,

    CHECK (sexo IN ('f', 'm'))
);
```

### 🔎 Destaques

A matrícula é a chave primária:

```sql
PRIMARY KEY
```

O nome, e-mail, sexo e data de nascimento são obrigatórios:

```sql
NOT NULL
```

O `CHECK` restringe o campo `sexo` aos valores:

```text
f
m
```

---

# 5. 👤 Criando a tabela responsável

```sql
CREATE TABLE responsavel (
    codResponsavel INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    cpf CHAR(14) NOT NULL,
    matricula INT NOT NULL UNIQUE,

    FOREIGN KEY (matricula)
        REFERENCES aluno(matricula)
);
```

O campo `matricula` estabelece o relacionamento com `aluno`.

A restrição:

```sql
UNIQUE
```

garante que uma matrícula não seja cadastrada novamente na tabela de responsáveis.

---

# 6. 👨‍🏫 Criando a tabela professor

```sql
CREATE TABLE professor (
    codProfessor INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    titulacao VARCHAR(50) NOT NULL
);
```

Exemplos de titulação:

```text
Graduação
Pós-Graduação
Mestrado
Doutorado
```

---

# 7. 📚 Criando a tabela disciplina

```sql
CREATE TABLE disciplina (
    codDisciplina INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL,
    cargaHr INT NOT NULL,
    cod_prof INT,

    FOREIGN KEY (cod_prof)
        REFERENCES professor(codProfessor)
);
```

O campo `cod_prof` pode ser `NULL`.

Isso permite cadastrar uma disciplina que ainda não possui professor associado.

---

# 8. 📝 Criando a tabela notas

A tabela `notas` relacionará:

- aluno;
- disciplina;
- ano letivo.

Além disso, serão armazenadas três notas.

```sql
CREATE TABLE notas (
    anoLetivo INT NOT NULL,
    cod_disc INT NOT NULL,
    mat_aluno INT NOT NULL,
    nota1 DECIMAL(4,2),
    nota2 DECIMAL(4,2),
    nota3 DECIMAL(4,2),

    PRIMARY KEY (anoLetivo, cod_disc, mat_aluno),

    FOREIGN KEY (cod_disc)
        REFERENCES disciplina(codDisciplina),

    FOREIGN KEY (mat_aluno)
        REFERENCES aluno(matricula)
);
```

---

# 9. 🔐 Chave primária composta

A tabela `notas` possui:

```sql
PRIMARY KEY (anoLetivo, cod_disc, mat_aluno)
```

Isso significa que a identificação do registro depende da combinação de três campos:

```text
Ano letivo
+
Disciplina
+
Aluno
```

Exemplo:

```text
2022 + Disciplina 2 + Aluno 5
```

Essa combinação identifica uma ocorrência específica de notas.

---

# 10. 🔎 Visualizando as tabelas

```sql
SHOW TABLES;
```

Para visualizar a estrutura de uma tabela:

```sql
DESCRIBE aluno;
```

```sql
DESCRIBE responsavel;
```

```sql
DESCRIBE professor;
```

```sql
DESCRIBE disciplina;
```

```sql
DESCRIBE notas;
```

---

# 11. 📥 Cadastrando alunos

```sql
INSERT INTO aluno
    (nome, email, sexo, dataNasc)
VALUES
    ('Leo', 'leo@uol.com', 'm', '1994-06-17'),
    ('Ana', 'ana@uol.com', 'f', '1998-03-29'),
    ('Bia', 'bia@aol.com', 'f', '1995-10-06'),
    ('Edu', 'edu@uol.com', 'm', '1990-08-21'),
    ('Nat', 'nat@uol.com', 'f', '1989-11-05'),
    ('Mel', 'mel@aol.com', 'f', '1985-04-30'),
    ('Rui', 'rui@uol.com', 'm', '1991-10-09');
```

Consultar:

```sql
SELECT * FROM aluno;
```

---

# 12. 👤 Cadastrando responsáveis

```sql
INSERT INTO responsavel
    (nome, cpf, matricula)
VALUES
    ('Pedro', '233.123.321-05', 5),
    ('Maria', '444.123.321-16', 2),
    ('Paulo', '064.123.321-41', 1),
    ('Bruna', '124.123.321-27', 4);
```

Consultar:

```sql
SELECT * FROM responsavel;
```

---

# 13. 👨‍🏫 Cadastrando professores

```sql
INSERT INTO professor
    (nome, titulacao)
VALUES
    ('Beto', 'Doutorado'),
    ('Rafa', 'Pos Graduacao'),
    ('Duda', 'Mestrado'),
    ('Cadu', 'Graduacao');
```

Consultar:

```sql
SELECT * FROM professor;
```

---

# 14. 📚 Cadastrando disciplinas

```sql
INSERT INTO disciplina
    (nome, cargaHr, cod_prof)
VALUES
    ('Logica', 32, 4),
    ('BD', 32, 1),
    ('PHP', 60, 2),
    ('SQL Server', 32, 2),
    ('BI', 40, 1),
    ('UML', 20, NULL),
    ('Oracle', 32, 1);
```

Consultar:

```sql
SELECT * FROM disciplina;
```

Observe que `UML` foi cadastrada sem professor:

```text
cod_prof = NULL
```

---

# 15. 📝 Cadastrando o primeiro registro de notas

```sql
INSERT INTO notas
    (anoLetivo, cod_disc, mat_aluno)
VALUES
    (2021, 3, 2);
```

Consultar:

```sql
SELECT * FROM notas;
```

Inicialmente as três notas estarão vazias.

---

# 16. ✏️ Atualizando as notas

Podemos lançar as notas individualmente.

### Nota 1

```sql
UPDATE notas
SET nota1 = 5.5
WHERE mat_aluno = 2
  AND cod_disc = 3
  AND anoLetivo = 2021;
```

### Nota 2

```sql
UPDATE notas
SET nota2 = 6
WHERE mat_aluno = 2
  AND cod_disc = 3
  AND anoLetivo = 2021;
```

### Nota 3

```sql
UPDATE notas
SET nota3 = 7
WHERE mat_aluno = 2
  AND cod_disc = 3
  AND anoLetivo = 2021;
```

Consultar:

```sql
SELECT * FROM notas;
```

---

# 17. ⚡ Atualizando várias colunas ao mesmo tempo

As três notas também podem ser alteradas em um único comando:

```sql
UPDATE notas
SET
    nota1 = 5.5,
    nota2 = 6,
    nota3 = 7
WHERE mat_aluno = 2
  AND cod_disc = 3
  AND anoLetivo = 2021;
```

Essa forma é mais prática quando várias colunas do mesmo registro precisam ser alteradas.

---

# 18. 📊 Inserindo várias avaliações

```sql
INSERT INTO notas
    (anoLetivo, mat_aluno, cod_disc, nota1, nota2, nota3)
VALUES
    (2021, 6, 4, 8, 7.5, 5),
    (2021, 3, 2, 4, 9.5, 7),
    (2021, 7, 1, 5, 7, 6),
    (2021, 3, 6, 4, 9, 6),
    (2021, 1, 3, 7, 6, 6),
    (2021, 2, 2, 3, 5, 7),
    (2022, 5, 4, 8, 7, 4),
    (2022, 3, 1, 6, 6, 8),
    (2022, 5, 2, 7, 4, 6),
    (2022, 7, 6, 8, 7, 5),
    (2022, 1, 3, 6, 5, 5),
    (2022, 2, 2, 5, 8, 7),
    (2022, 3, 5, 5, 5.5, 5),
    (2022, 4, 1, 7, 8.5, 6),
    (2022, 6, 2, 6.5, 8.5, 8),
    (2022, 5, 3, 5.5, 5, 8);
```

Consultar:

```sql
SELECT * FROM notas;
```

---

# 19. 🔗 Consultando aluno e responsável

```sql
SELECT
    a.matricula,
    a.nome AS aluno,
    a.email,
    r.nome AS responsavel,
    r.cpf
FROM aluno AS a
INNER JOIN responsavel AS r
    ON a.matricula = r.matricula;
```

Essa consulta apresenta somente alunos que possuem responsável cadastrado.

---

# 20. 👨‍🏫 Consultando professores e disciplinas

```sql
SELECT
    p.nome AS professor,
    p.titulacao,
    d.nome AS disciplina,
    d.cargaHr
FROM professor AS p
INNER JOIN disciplina AS d
    ON p.codProfessor = d.cod_prof;
```

---

# 21. 📚 Encontrando disciplinas sem professor

Como `cod_prof` pode ser `NULL`, podemos localizar disciplinas sem professor usando `LEFT JOIN`.

```sql
SELECT
    d.codDisciplina,
    d.nome,
    d.cargaHr
FROM disciplina AS d
LEFT JOIN professor AS p
    ON d.cod_prof = p.codProfessor
WHERE p.codProfessor IS NULL;
```

---

# 22. 👨‍🏫 Encontrando professores sem disciplina

```sql
SELECT
    p.codProfessor,
    p.nome,
    p.titulacao
FROM professor AS p
LEFT JOIN disciplina AS d
    ON p.codProfessor = d.cod_prof
WHERE d.codDisciplina IS NULL;
```

---

# 23. 📊 Consultando notas com o nome do aluno

```sql
SELECT
    a.nome AS aluno,
    n.anoLetivo,
    n.nota1,
    n.nota2,
    n.nota3
FROM aluno AS a
INNER JOIN notas AS n
    ON a.matricula = n.mat_aluno
ORDER BY a.nome;
```

---

# 24. 📚 Consultando aluno, disciplina e notas

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    n.anoLetivo,
    n.nota1,
    n.nota2,
    n.nota3
FROM notas AS n
INNER JOIN aluno AS a
    ON n.mat_aluno = a.matricula
INNER JOIN disciplina AS d
    ON n.cod_disc = d.codDisciplina
ORDER BY a.nome, n.anoLetivo;
```

---

# 25. 🧮 Calculando a média

Podemos calcular a média das três avaliações:

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    n.anoLetivo,
    n.nota1,
    n.nota2,
    n.nota3,
    ROUND((n.nota1 + n.nota2 + n.nota3) / 3, 2) AS media
FROM notas AS n
INNER JOIN aluno AS a
    ON n.mat_aluno = a.matricula
INNER JOIN disciplina AS d
    ON n.cod_disc = d.codDisciplina;
```

---

# 26. ✅ Verificando aprovação

Podemos utilizar `CASE` para classificar o resultado.

Neste exemplo será considerada média **maior ou igual a 6** como aprovação.

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    n.anoLetivo,
    ROUND((n.nota1 + n.nota2 + n.nota3) / 3, 2) AS media,

    CASE
        WHEN (n.nota1 + n.nota2 + n.nota3) / 3 >= 6
            THEN 'Aprovado'
        ELSE 'Reprovado'
    END AS situacao

FROM notas AS n
INNER JOIN aluno AS a
    ON n.mat_aluno = a.matricula
INNER JOIN disciplina AS d
    ON n.cod_disc = d.codDisciplina;
```

> A regra de média utilizada acima é uma regra didática para o laboratório. O material-base informa que as três notas serão utilizadas para gerar a média e verificar a condição de aprovação, mas não define uma média mínima específica. fileciteturn23file0L18-L19

---

# 27. 🔎 Consultando somente aprovados

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    ROUND((n.nota1 + n.nota2 + n.nota3) / 3, 2) AS media
FROM notas AS n
INNER JOIN aluno AS a
    ON n.mat_aluno = a.matricula
INNER JOIN disciplina AS d
    ON n.cod_disc = d.codDisciplina
WHERE (n.nota1 + n.nota2 + n.nota3) / 3 >= 6;
```

---

# 28. 🔎 Consultando somente reprovados

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    ROUND((n.nota1 + n.nota2 + n.nota3) / 3, 2) AS media
FROM notas AS n
INNER JOIN aluno AS a
    ON n.mat_aluno = a.matricula
INNER JOIN disciplina AS d
    ON n.cod_disc = d.codDisciplina
WHERE (n.nota1 + n.nota2 + n.nota3) / 3 < 6;
```

---

# 29. 📈 Média por disciplina

```sql
SELECT
    d.nome AS disciplina,
    ROUND(AVG((n.nota1 + n.nota2 + n.nota3) / 3), 2) AS mediaTurma
FROM notas AS n
INNER JOIN disciplina AS d
    ON n.cod_disc = d.codDisciplina
GROUP BY d.codDisciplina, d.nome
ORDER BY d.nome;
```

---

# 30. 📅 Consultando um ano letivo específico

Para consultar somente as avaliações de 2022:

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    n.nota1,
    n.nota2,
    n.nota3
FROM notas AS n
INNER JOIN aluno AS a
    ON n.mat_aluno = a.matricula
INNER JOIN disciplina AS d
    ON n.cod_disc = d.codDisciplina
WHERE n.anoLetivo = 2022
ORDER BY a.nome;
```

---

# 31. 🧪 Laboratório de consultas

Execute as consultas abaixo para reforçar o conteúdo.

### Todos os alunos

```sql
SELECT * FROM aluno;
```

### Todos os responsáveis

```sql
SELECT * FROM responsavel;
```

### Todos os professores

```sql
SELECT * FROM professor;
```

### Todas as disciplinas

```sql
SELECT * FROM disciplina;
```

### Todas as notas

```sql
SELECT * FROM notas;
```

### Alunos com seus responsáveis

```sql
SELECT
    a.nome AS aluno,
    r.nome AS responsavel
FROM aluno a
INNER JOIN responsavel r
    ON a.matricula = r.matricula;
```

### Professores e disciplinas

```sql
SELECT
    p.nome AS professor,
    d.nome AS disciplina
FROM professor p
INNER JOIN disciplina d
    ON p.codProfessor = d.cod_prof;
```

### Alunos, disciplinas e médias

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    ROUND((n.nota1 + n.nota2 + n.nota3) / 3, 2) AS media
FROM notas n
INNER JOIN aluno a
    ON n.mat_aluno = a.matricula
INNER JOIN disciplina d
    ON n.cod_disc = d.codDisciplina;
```

---

# 32. 📝 Exercícios

## Exercício 01

Liste todos os alunos cadastrados.

---

## Exercício 02

Liste os alunos que possuem responsável cadastrado.

---

## Exercício 03

Liste os professores e suas respectivas disciplinas.

---

## Exercício 04

Identifique as disciplinas que ainda não possuem professor.

---

## Exercício 05

Identifique os professores que não possuem disciplinas associadas.

---

## Exercício 06

Liste todos os registros de notas com o nome do aluno.

---

## Exercício 07

Liste o aluno, a disciplina e as três notas obtidas.

---

## Exercício 08

Calcule a média das três avaliações de cada aluno.

---

## Exercício 09

Classifique cada registro como:

```text
Aprovado
Reprovado
```

utilizando `CASE`.

---

## Exercício 10

Apresente somente os alunos com média igual ou superior a 6.

---

## Exercício 11

Apresente somente os alunos com média inferior a 6.

---

## Exercício 12

Calcule a média geral de cada disciplina.

---

## Exercício 13

Mostre somente os registros referentes ao ano letivo de 2022.

---

## Exercício 14

Crie uma consulta que apresente:

```text
Aluno
Disciplina
Professor
Nota 1
Nota 2
Nota 3
Média
Situação
```

---

# 33. 🧠 Conceitos importantes

### `PRIMARY KEY`

Identifica unicamente cada registro.

```sql
PRIMARY KEY
```

### `FOREIGN KEY`

Cria um relacionamento entre tabelas.

```sql
FOREIGN KEY (...) REFERENCES ...
```

### `UNIQUE`

Impede valores duplicados em uma coluna ou conjunto de colunas.

```sql
UNIQUE
```

### `CHECK`

Restringe os valores aceitos conforme uma regra.

```sql
CHECK (sexo IN ('f', 'm'))
```

### Chave composta

Utiliza mais de uma coluna para identificar um registro.

```sql
PRIMARY KEY (anoLetivo, cod_disc, mat_aluno)
```

---

# 34. 📌 Resumo da aula

Nesta aula foi desenvolvido um modelo de banco de dados para controle acadêmico.

O modelo possui cinco entidades principais:

```text
ALUNO
RESPONSAVEL
PROFESSOR
DISCIPLINA
NOTAS
```

Também foram praticados:

```text
CREATE DATABASE
CREATE TABLE
PRIMARY KEY
FOREIGN KEY
UNIQUE
CHECK
INSERT
UPDATE
SELECT
INNER JOIN
LEFT JOIN
WHERE
GROUP BY
AVG()
ROUND()
CASE
```

A tabela `notas` utiliza uma chave primária composta formada por:

```text
anoLetivo
cod_disc
mat_aluno
```

Esse modelo permite registrar as avaliações de um aluno em uma disciplina durante determinado ano letivo.

---

# 35. ✅ Checklist da Aula 07

- [ ] Criar o banco `aula7`.
- [ ] Criar a tabela `aluno`.
- [ ] Criar a tabela `responsavel`.
- [ ] Criar a tabela `professor`.
- [ ] Criar a tabela `disciplina`.
- [ ] Criar a tabela `notas`.
- [ ] Utilizar `PRIMARY KEY`.
- [ ] Utilizar `FOREIGN KEY`.
- [ ] Utilizar `UNIQUE`.
- [ ] Utilizar `CHECK`.
- [ ] Trabalhar com chave composta.
- [ ] Inserir alunos.
- [ ] Inserir responsáveis.
- [ ] Inserir professores.
- [ ] Inserir disciplinas.
- [ ] Inserir notas.
- [ ] Alterar notas com `UPDATE`.
- [ ] Utilizar `INNER JOIN`.
- [ ] Utilizar `LEFT JOIN`.
- [ ] Calcular médias.
- [ ] Utilizar `CASE`.
- [ ] Identificar aprovados e reprovados.

---

# 🚀 Próxima etapa

Na próxima aula, o modelo poderá ser utilizado para aprofundar consultas SQL, cálculos, filtros e operações sobre os dados acadêmicos.
