# 🏆 Gabarito — Projeto Final de Banco de Dados MySQL

**Professor:** Ygor Silva  
**Curso:** Banco de Dados — MySQL  
**Projeto:** Sistema de Controle Acadêmico  
**Finalidade:** Gabarito da atividade conclusiva

---

# 1. 🎯 Proposta do projeto

O projeto consiste na criação de um banco de dados para uma instituição de ensino.

O sistema deverá controlar:

- alunos;
- responsáveis;
- professores;
- disciplinas;
- notas;
- ano letivo;
- médias;
- situação acadêmica.

---

# 2. 🧩 Modelo do sistema

O relacionamento principal será:

```text
ALUNO
  │
  ├──────── RESPONSAVEL
  │
  └──────── NOTAS ──────── DISCIPLINA
                              │
                              │
                          PROFESSOR
```

---

# 3. 🗄️ Criação do banco

```sql
DROP DATABASE IF EXISTS projeto_final;

CREATE DATABASE projeto_final;

USE projeto_final;
```

---

# 4. 👨‍🎓 Tabela ALUNO

```sql
CREATE TABLE aluno (
    matricula INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    email VARCHAR(60) NOT NULL UNIQUE,
    sexo CHAR(1) NOT NULL,
    data_nascimento DATE NOT NULL,

    CHECK (sexo IN ('M', 'F'))
);
```

---

# 5. 👤 Tabela RESPONSAVEL

```sql
CREATE TABLE responsavel (
    cod_responsavel INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    cpf CHAR(14) NOT NULL UNIQUE,
    matricula INT NOT NULL UNIQUE,

    CONSTRAINT fk_responsavel_aluno
        FOREIGN KEY (matricula)
        REFERENCES aluno(matricula)
);
```

O `UNIQUE` na matrícula permite representar, neste projeto, no máximo um responsável cadastrado para cada aluno.

---

# 6. 👨‍🏫 Tabela PROFESSOR

```sql
CREATE TABLE professor (
    cod_professor INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    titulacao VARCHAR(50) NOT NULL
);
```

---

# 7. 📚 Tabela DISCIPLINA

```sql
CREATE TABLE disciplina (
    cod_disciplina INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL,
    carga_horaria INT NOT NULL,
    cod_professor INT,

    CONSTRAINT fk_disciplina_professor
        FOREIGN KEY (cod_professor)
        REFERENCES professor(cod_professor)
);
```

O professor pode ser `NULL`, permitindo cadastrar uma disciplina ainda sem professor.

---

# 8. 📝 Tabela NOTAS

```sql
CREATE TABLE notas (
    ano_letivo INT NOT NULL,
    cod_disciplina INT NOT NULL,
    matricula INT NOT NULL,

    nota1 DECIMAL(4,2),
    nota2 DECIMAL(4,2),
    nota3 DECIMAL(4,2),

    PRIMARY KEY (
        ano_letivo,
        cod_disciplina,
        matricula
    ),

    CONSTRAINT fk_notas_disciplina
        FOREIGN KEY (cod_disciplina)
        REFERENCES disciplina(cod_disciplina),

    CONSTRAINT fk_notas_aluno
        FOREIGN KEY (matricula)
        REFERENCES aluno(matricula),

    CHECK (nota1 IS NULL OR (nota1 >= 0 AND nota1 <= 10)),
    CHECK (nota2 IS NULL OR (nota2 >= 0 AND nota2 <= 10)),
    CHECK (nota3 IS NULL OR (nota3 >= 0 AND nota3 <= 10))
);
```

---

# 9. 📥 Inserindo alunos

```sql
INSERT INTO aluno
    (nome, email, sexo, data_nascimento)
VALUES
    ('Ana Silva', 'ana@email.com', 'F', '1998-03-29'),
    ('Bruno Souza', 'bruno@email.com', 'M', '1997-08-14'),
    ('Carla Mendes', 'carla@email.com', 'F', '1999-01-20'),
    ('Daniel Lima', 'daniel@email.com', 'M', '1996-11-05'),
    ('Eduarda Alves', 'eduarda@email.com', 'F', '1998-07-12'),
    ('Felipe Costa', 'felipe@email.com', 'M', '1995-04-30'),
    ('Gabriela Rocha', 'gabriela@email.com', 'F', '2000-02-18'),
    ('Henrique Martins', 'henrique@email.com', 'M', '1997-10-09');
```

Verificação:

```sql
SELECT * FROM aluno;
```

---

# 10. 👤 Inserindo responsáveis

```sql
INSERT INTO responsavel
    (nome, cpf, matricula)
VALUES
    ('Maria Silva', '111.111.111-11', 1),
    ('José Souza', '222.222.222-22', 2),
    ('Paulo Mendes', '333.333.333-33', 3),
    ('Luciana Lima', '444.444.444-44', 4);
```

Verificação:

```sql
SELECT * FROM responsavel;
```

---

# 11. 👨‍🏫 Inserindo professores

```sql
INSERT INTO professor
    (nome, titulacao)
VALUES
    ('Carlos Oliveira', 'Doutorado'),
    ('Mariana Santos', 'Mestrado'),
    ('Roberto Alves', 'Pos-Graduacao'),
    ('Patricia Gomes', 'Mestrado');
```

Verificação:

```sql
SELECT * FROM professor;
```

---

# 12. 📚 Inserindo disciplinas

```sql
INSERT INTO disciplina
    (nome, carga_horaria, cod_professor)
VALUES
    ('Banco de Dados', 60, 1),
    ('Programacao', 60, 2),
    ('Redes de Computadores', 40, 3),
    ('Engenharia de Software', 40, 4),
    ('Sistemas Operacionais', 40, NULL);
```

Verificação:

```sql
SELECT * FROM disciplina;
```

---

# 13. 📝 Inserindo notas

```sql
INSERT INTO notas
    (ano_letivo, cod_disciplina, matricula, nota1, nota2, nota3)
VALUES
    (2026, 1, 1, 8.0, 7.5, 9.0),
    (2026, 2, 1, 7.0, 8.0, 7.5),
    (2026, 3, 1, 6.0, 7.0, 6.5),

    (2026, 1, 2, 5.0, 6.0, 5.5),
    (2026, 2, 2, 8.0, 9.0, 8.5),
    (2026, 4, 2, 7.0, 7.5, 8.0),

    (2026, 1, 3, 9.0, 8.5, 9.5),
    (2026, 3, 3, 8.0, 8.5, 9.0),

    (2026, 1, 4, 4.0, 5.0, 5.5),
    (2026, 2, 4, 6.0, 5.5, 6.0),

    (2026, 2, 5, 9.0, 9.5, 8.5),
    (2026, 4, 5, 8.0, 7.5, 9.0),

    (2026, 1, 6, 7.0, 6.5, 7.5),
    (2026, 3, 6, 5.0, 6.0, 5.5),

    (2026, 2, 7, 8.0, 8.5, 9.0),
    (2026, 4, 7, 9.0, 8.0, 8.5),

    (2026, 1, 8, 6.0, 6.5, 7.0),
    (2026, 3, 8, 7.0, 7.5, 7.0);
```

---

# 14. 🔎 Consulta 01 — Todos os alunos

```sql
SELECT *
FROM aluno;
```

---

# 15. 🔎 Consulta 02 — Alunos do sexo feminino

```sql
SELECT
    matricula,
    nome,
    email
FROM aluno
WHERE sexo = 'F'
ORDER BY nome;
```

---

# 16. 🔎 Consulta 03 — Alunos e responsáveis

```sql
SELECT
    a.matricula,
    a.nome AS aluno,
    r.nome AS responsavel,
    r.cpf
FROM aluno AS a
INNER JOIN responsavel AS r
    ON a.matricula = r.matricula
ORDER BY a.nome;
```

---

# 17. 🔎 Consulta 04 — Professores e disciplinas

```sql
SELECT
    p.nome AS professor,
    p.titulacao,
    d.nome AS disciplina,
    d.carga_horaria
FROM professor AS p
INNER JOIN disciplina AS d
    ON p.cod_professor = d.cod_professor
ORDER BY p.nome;
```

---

# 18. 🔎 Consulta 05 — Disciplinas sem professor

```sql
SELECT
    d.cod_disciplina,
    d.nome,
    d.carga_horaria
FROM disciplina AS d
LEFT JOIN professor AS p
    ON d.cod_professor = p.cod_professor
WHERE p.cod_professor IS NULL;
```

**Resultado esperado:** `Sistemas Operacionais`.

---

# 19. 📊 Consulta 06 — Notas dos alunos

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,
    n.ano_letivo,
    n.nota1,
    n.nota2,
    n.nota3
FROM notas AS n
INNER JOIN aluno AS a
    ON n.matricula = a.matricula
INNER JOIN disciplina AS d
    ON n.cod_disciplina = d.cod_disciplina
ORDER BY a.nome, d.nome;
```

---

# 20. 🧮 Consulta 07 — Cálculo da média

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,

    ROUND(
        (n.nota1 + n.nota2 + n.nota3) / 3,
        2
    ) AS media

FROM notas AS n

INNER JOIN aluno AS a
    ON n.matricula = a.matricula

INNER JOIN disciplina AS d
    ON n.cod_disciplina = d.cod_disciplina

ORDER BY a.nome;
```

---

# 21. ✅ Consulta 08 — Situação acadêmica

Considerando média mínima igual a **6,00**:

```sql
SELECT
    a.nome AS aluno,
    d.nome AS disciplina,

    ROUND(
        (n.nota1 + n.nota2 + n.nota3) / 3,
        2
    ) AS media,

    CASE
        WHEN (n.nota1 + n.nota2 + n.nota3) / 3 >= 6
            THEN 'Aprovado'
        ELSE 'Reprovado'
    END AS situacao

FROM notas AS n

INNER JOIN aluno AS a
    ON n.matricula = a.matricula

INNER JOIN disciplina AS d
    ON n.cod_disciplina = d.cod_disciplina

ORDER BY a.nome, d.nome;
```

---

# 22. 📈 Consulta 09 — Média geral por disciplina

```sql
SELECT
    d.nome AS disciplina,

    ROUND(
        AVG(
            (n.nota1 + n.nota2 + n.nota3) / 3
        ),
        2
    ) AS media_turma

FROM notas AS n

INNER JOIN disciplina AS d
    ON n.cod_disciplina = d.cod_disciplina

GROUP BY
    d.cod_disciplina,
    d.nome

ORDER BY media_turma DESC;
```

---

# 23. 🔢 Consulta 10 — Quantidade de alunos por disciplina

```sql
SELECT
    d.nome AS disciplina,
    COUNT(DISTINCT n.matricula) AS quantidade_alunos

FROM disciplina AS d

LEFT JOIN notas AS n
    ON d.cod_disciplina = n.cod_disciplina

GROUP BY
    d.cod_disciplina,
    d.nome

ORDER BY quantidade_alunos DESC;
```

---

# 24. 🏆 Consulta 11 — Melhor média por disciplina

```sql
SELECT
    d.nome AS disciplina,

    MAX(
        (n.nota1 + n.nota2 + n.nota3) / 3
    ) AS maior_media

FROM notas AS n

INNER JOIN disciplina AS d
    ON n.cod_disciplina = d.cod_disciplina

GROUP BY
    d.cod_disciplina,
    d.nome;
```

---

# 25. 📉 Consulta 12 — Menor média por disciplina

```sql
SELECT
    d.nome AS disciplina,

    MIN(
        (n.nota1 + n.nota2 + n.nota3) / 3
    ) AS menor_media

FROM notas AS n

INNER JOIN disciplina AS d
    ON n.cod_disciplina = d.cod_disciplina

GROUP BY
    d.cod_disciplina,
    d.nome;
```

---

# 26. 🧮 Consulta 13 — Quantidade de aprovados e reprovados

```sql
SELECT
    CASE
        WHEN (nota1 + nota2 + nota3) / 3 >= 6
            THEN 'Aprovado'
        ELSE 'Reprovado'
    END AS situacao,

    COUNT(*) AS quantidade

FROM notas

GROUP BY situacao;
```

---

# 27. 🥇 Consulta 14 — Ranking dos alunos

```sql
SELECT
    a.nome AS aluno,

    ROUND(
        AVG(
            (n.nota1 + n.nota2 + n.nota3) / 3
        ),
        2
    ) AS media_geral

FROM aluno AS a

INNER JOIN notas AS n
    ON a.matricula = n.matricula

GROUP BY
    a.matricula,
    a.nome

ORDER BY media_geral DESC;
```

---

# 28. 🔍 Consulta 15 — Alunos com média geral igual ou superior a 7

```sql
SELECT
    a.nome AS aluno,

    ROUND(
        AVG(
            (n.nota1 + n.nota2 + n.nota3) / 3
        ),
        2
    ) AS media_geral

FROM aluno AS a

INNER JOIN notas AS n
    ON a.matricula = n.matricula

GROUP BY
    a.matricula,
    a.nome

HAVING media_geral >= 7

ORDER BY media_geral DESC;
```

---

# 29. 📋 Consulta final — Relatório acadêmico completo

Esta é uma das principais consultas do projeto.

```sql
SELECT
    a.matricula,
    a.nome AS aluno,
    d.nome AS disciplina,
    p.nome AS professor,

    n.nota1,
    n.nota2,
    n.nota3,

    ROUND(
        (n.nota1 + n.nota2 + n.nota3) / 3,
        2
    ) AS media,

    CASE
        WHEN (n.nota1 + n.nota2 + n.nota3) / 3 >= 6
            THEN 'Aprovado'
        ELSE 'Reprovado'
    END AS situacao

FROM notas AS n

INNER JOIN aluno AS a
    ON n.matricula = a.matricula

INNER JOIN disciplina AS d
    ON n.cod_disciplina = d.cod_disciplina

LEFT JOIN professor AS p
    ON d.cod_professor = p.cod_professor

ORDER BY
    a.nome,
    d.nome;
```

---

# 30. 📌 Gabarito conceitual

O projeto atende aos requisitos mínimos:

| Requisito | Implementação |
|---|---|
| Banco de dados | `projeto_final` |
| Tabelas | 5 |
| Chave primária | Sim |
| Chave estrangeira | Sim |
| `UNIQUE` | Sim |
| `CHECK` | Sim |
| Relacionamentos | Sim |
| `INSERT` | Sim |
| `UPDATE` | Disponível para alterações |
| `SELECT` | Sim |
| `WHERE` | Sim |
| `JOIN` | Sim |
| `LEFT JOIN` | Sim |
| `GROUP BY` | Sim |
| `HAVING` | Sim |
| `COUNT()` | Sim |
| `AVG()` | Sim |
| `MIN()` | Sim |
| `MAX()` | Sim |
| `CASE` | Sim |
| Cálculo de média | Sim |

---

# 31. 🧪 Teste de integridade

Tente inserir um sexo inválido:

```sql
INSERT INTO aluno
    (nome, email, sexo, data_nascimento)
VALUES
    ('Teste', 'teste@email.com', 'X', '2000-01-01');
```

O banco deverá rejeitar o registro por causa da restrição:

```sql
CHECK (sexo IN ('M', 'F'))
```

---

# 32. 🔐 Teste de chave estrangeira

Tente cadastrar um responsável para uma matrícula inexistente:

```sql
INSERT INTO responsavel
    (nome, cpf, matricula)
VALUES
    ('Teste', '555.555.555-55', 999);
```

O banco deverá rejeitar o registro porque a matrícula `999` não existe em `aluno`.

---

# 33. 🔑 Teste da chave composta

Tente cadastrar novamente o mesmo aluno, disciplina e ano:

```sql
INSERT INTO notas
    (ano_letivo, cod_disciplina, matricula, nota1, nota2, nota3)
VALUES
    (2026, 1, 1, 9, 9, 9);
```

O registro deverá ser rejeitado porque a combinação:

```text
ano_letivo
+
cod_disciplina
+
matricula
```

já existe.

---

# 34. 🏁 Resultado esperado

Ao executar o projeto, o aluno deverá demonstrar que consegue:

```text
Criar o banco
      ↓
Criar as tabelas
      ↓
Definir as chaves
      ↓
Criar relacionamentos
      ↓
Inserir dados
      ↓
Alterar dados
      ↓
Consultar dados
      ↓
Relacionar tabelas
      ↓
Calcular médias
      ↓
Classificar resultados
      ↓
Gerar relatório
```

---

# 35. 🎓 Critério de conclusão

O projeto pode ser considerado concluído quando:

- [ ] Banco criado sem erros.
- [ ] Todas as tabelas criadas.
- [ ] Relacionamentos funcionando.
- [ ] Dados cadastrados.
- [ ] Consultas executadas corretamente.
- [ ] Médias calculadas.
- [ ] Situação acadêmica apresentada.
- [ ] Relatório final funcionando.
- [ ] DER/MER disponível.
- [ ] Script SQL organizado.
- [ ] Projeto documentado no GitHub.

---

# 🏆 Conclusão

Este gabarito apresenta uma implementação completa de referência para o projeto final do curso.

O objetivo não é apenas reproduzir os comandos, mas compreender a lógica utilizada para transformar um problema acadêmico em um banco de dados relacional.

A estrutura pode ser ampliada posteriormente com novas tabelas, regras de negócio, consultas avançadas, `VIEW`, índices, procedimentos armazenados, `TRIGGER`, usuários, permissões e recursos de administração do MySQL.

**Professor: Ygor Silva**

**Curso de Banco de Dados — MySQL**
