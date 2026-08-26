# 🗄️ Aula 05 — FN2, FN3 e Relacionamentos no MySQL

**Curso:** Banco de Dados — MySQL  
**Aula:** 05  
**Tema:** Segunda e Terceira Forma Normal, relacionamentos e tabela associativa

---

## 🎯 Objetivos

Nesta aula vamos estudar:

- Segunda Forma Normal (FN2);
- dependência parcial;
- Terceira Forma Normal (FN3);
- dependência transitiva;
- relacionamentos entre entidades;
- chave primária composta;
- relacionamento entre funcionário e cargo;
- relacionamento entre funcionário e cônjuge;
- relacionamento entre funcionário e projeto;
- utilização de tabela associativa para relacionamento entre funcionários e projetos.

---

# 1. 🧠 Segunda Forma Normal — FN2

A **Segunda Forma Normal (FN2)** determina que os campos que não fazem parte da chave primária devem estar diretamente relacionados à chave primária da tabela.

Um dos principais pontos da FN2 aparece quando uma tabela possui **chave primária composta**.

Nesse caso, não deve existir **dependência parcial**.

### Dependência parcial

A dependência parcial ocorre quando um atributo que não participa da chave primária depende somente de uma parte da chave composta.

Exemplo conceitual:

```text
PK = (codFuncionario, codProjeto)

valorProjeto
     ↓
depende somente de codProjeto
```

Nesse cenário, `valorProjeto` não depende da combinação completa da chave.

A solução é separar as informações em tabelas adequadas.

---

# 2. 🧠 Terceira Forma Normal — FN3

A **Terceira Forma Normal (FN3)** trata da dependência transitiva.

Não deve existir uma situação em que um campo que não participa da chave primária dependa de outro campo que também não participa da chave primária.

### Exemplo

Considere:

```text
Funcionario
    ↓
codCargo
    ↓
titulo
salario
```

Se funcionários que possuem o mesmo cargo recebem o mesmo salário, manter `titulo` e `salario` diretamente na tabela `Funcionario` gera uma dependência que pode ser melhor representada por uma tabela própria.

A solução utilizada no modelo é criar a entidade:

```text
Cargo
```

e relacioná-la com:

```text
Funcionario
```

---

# 3. 🏢 Estudo de caso

O cenário da aula representa uma **empresa** que precisa armazenar informações sobre seus funcionários.

São necessários os seguintes dados:

### Funcionário

- nome;
- CPF;
- gênero;
- estado civil;
- cargo.

### Cargo

- título;
- salário.

Funcionários que exercem o mesmo cargo recebem o mesmo salário.

Além disso:

- funcionários podem informar o nome do cônjuge;
- também pode ser registrado o telefone do cônjuge;
- funcionários podem atuar em projetos;
- projetos possuem nome;
- projetos possuem valor;
- projetos possuem data de início;
- projetos possuem tempo previsto para término;
- projetos possuem porcentagem de participação;
- a porcentagem de participação é aplicada ao valor do projeto para gerar um bônus.

---

# 4. 📐 Modelo de Entidades e Relacionamentos — MER

O modelo conceitual apresenta quatro entidades principais:

```text
FUNCIONARIO
     │
     ├──────── CARGO
     │
     ├──────── CONJUGE
     │
     └──────── PROJETO
```

No relacionamento com projetos existe uma situação de **muitos para muitos**:

```text
FUNCIONARIO  N : N  PROJETO
```

Por isso, no modelo relacional é necessária uma tabela associativa.

---

## 🖼️ MER da aula

![Modelo de Entidades e Relacionamentos](../imagens/MER%20Aula%205%20%28Func%20x%20Proj%29.jpg)

---

# 5. 🧩 Entidade Funcionário

A entidade `Funcionario` representa os empregados da empresa.

Campos apresentados no modelo:

| Campo | Tipo |
|---|---|
| `codFuncionario` | INTEGER |
| `nome` | VARCHAR(80) |
| `cpf` | CHAR(14) |
| `sexo` | CHAR(1) |
| `estadoCivil` | VARCHAR(30) |
| `codCargo` | INTEGER |

A chave primária é:

```text
codFuncionario
```

O campo:

```text
codCargo
```

faz a ligação do funcionário com a entidade `Cargo`.

---

# 6. 💼 Entidade Cargo

A entidade `Cargo` concentra informações que são compartilhadas pelos funcionários que ocupam o mesmo cargo.

Campos apresentados no modelo:

| Campo | Tipo |
|---|---|
| `codCargo` | INTEGER |
| `titulo` | VARCHAR(70) |
| `salario` | DECIMAL(10,2) |

Chave primária:

```text
codCargo
```

A separação do cargo evita repetir informações como título e salário em todos os registros de funcionários.

---

# 7. 💍 Entidade Cônjuge

A entidade `Conjuge` armazena informações do cônjuge informado pelo funcionário.

Campos apresentados:

| Campo | Tipo |
|---|---|
| `codConjuge` | INTEGER |
| `nome` | VARCHAR(80) |
| `telefone` | VARCHAR(20) |
| `codFuncionario` | INTEGER |

Chave primária:

```text
codConjuge
```

A ligação com funcionário é realizada por:

```text
codFuncionario
```

---

# 8. 📋 Entidade Projeto

A entidade `Projeto` representa os projetos da empresa.

Campos apresentados:

| Campo | Tipo |
|---|---|
| `codProjeto` | INTEGER |
| `nome` | VARCHAR(80) |
| `valor` | DECIMAL(10,2) |
| `dataInicio` | DATE |
| `tempPrev` | INTEGER |
| `porcent` | FLOAT |

Chave primária:

```text
codProjeto
```

A porcentagem de participação está relacionada ao bônus gerado para o funcionário que atua no projeto.

---

# 9. 🔗 Relacionamento Funcionário × Cargo

O relacionamento apresentado no modelo é:

```text
Cargo 1 ───────── N Funcionario
```

Um cargo pode estar associado a vários funcionários.

Cada funcionário está associado a um cargo.

Representação:

```text
CARGO
  │
  │ 1
  │
  └──────── N
             FUNCIONARIO
```

No modelo relacional:

```text
Funcionario.codCargo
        ↓
Cargo.codCargo
```

---

# 10. 💍 Relacionamento Funcionário × Cônjuge

O modelo apresenta o relacionamento:

```text
Funcionario 1 ───── 0..1 Conjuge
```

Isso significa que:

- um funcionário pode não informar cônjuge;
- um funcionário pode possuir um cônjuge cadastrado;
- o cadastro do cônjuge está associado ao funcionário.

A cardinalidade apresentada no modelo é:

```text
Funcionario (1,1)
Conjuge    (0,1)
```

---

# 11. 📊 Relacionamento Funcionário × Projeto

O modelo apresenta o relacionamento:

```text
Funcionario N ───── N Projeto
```

Um funcionário pode atuar em vários projetos.

Um projeto pode possuir vários funcionários.

Esse é um relacionamento **muitos para muitos**.

Em um banco relacional, esse tipo de relacionamento precisa ser transformado em uma tabela associativa.

---

# 12. 🔑 Chave primária composta

A tabela associativa apresentada no modelo é:

```text
alocado
```

Ela possui:

```text
codFuncionario
codProjeto
```

Os dois campos formam uma **chave primária composta**.

Representação:

```text
PRIMARY KEY (
    codFuncionario,
    codProjeto
)
```

A combinação dos dois valores identifica uma participação específica de um funcionário em um projeto.

---

# 13. 🧱 Tabela ALocado

A estrutura apresentada no DER é:

| Campo | Tipo | Função |
|---|---|---|
| `codFuncionario` | INTEGER | Funcionário |
| `codProjeto` | INTEGER | Projeto |

Chave primária composta:

```text
(codFuncionario, codProjeto)
```

Relacionamentos:

```text
codFuncionario → Funcionario
codProjeto     → Projeto
```

---

# 14. 🖼️ Modelo de Entidades e Relacionamentos

O modelo lógico apresenta a transformação das entidades e relacionamentos para tabelas.

![Diagrama de Entidades e Relacionamentos](../imagens/DER%20Aula%205%20%28Func%20x%20Proj%29.jpg)

---

# 15. 🔄 Transformação MER → Modelo Relacional

A estrutura pode ser visualizada desta forma:

```text
Cargo
  │
  └── codCargo
          │
          ▼
Funcionario
  │
  ├── codFuncionario
  ├── nome
  ├── cpf
  ├── sexo
  ├── estadoCivil
  └── codCargo
       │
       ├──────────► Conjuge
       │
       └──────────► Alocado ◄──────── Projeto
```

A tabela `Alocado` resolve o relacionamento muitos-para-muitos entre funcionários e projetos.

---

# 16. 🧮 Por que utilizar uma tabela associativa?

Imagine:

```text
Funcionário 01 → Projeto A
Funcionário 01 → Projeto B
Funcionário 02 → Projeto A
Funcionário 03 → Projeto A
Funcionário 03 → Projeto C
```

Não seria adequado criar várias colunas de projetos dentro da tabela `Funcionario`.

Também não seria adequado armazenar vários funcionários em uma única coluna de `Projeto`.

A tabela associativa permite representar corretamente as relações:

```text
Funcionario       Alocado       Projeto
     01              A
     01              B
     02              A
     03              A
     03              C
```

---

# 17. 🧠 FN2 aplicada ao relacionamento

A tabela `Alocado` possui uma chave composta:

```text
(codFuncionario, codProjeto)
```

Por isso devemos verificar se os atributos existentes dependem da combinação completa.

No modelo apresentado, os campos da associação são identificados pelos dois códigos.

Isso evita colocar informações do funcionário ou do projeto dentro da tabela associativa.

---

# 18. 🧠 FN3 aplicada ao modelo

A FN3 ajuda a separar informações que pertencem a outras entidades.

Por exemplo:

```text
Funcionario
     │
     └── codCargo
             │
             ▼
           Cargo
           título
           salário
```

Assim, o salário fica associado ao cargo e não precisa ser repetido em todos os funcionários que ocupam aquele cargo.

---

# 19. 🗃️ Exemplo de estrutura SQL

A estrutura abaixo representa o modelo apresentado nos diagramas.

## Cargo

```sql
CREATE TABLE Cargo (
    codCargo INTEGER PRIMARY KEY,
    titulo VARCHAR(70),
    salario DECIMAL(10,2)
);
```

## Funcionario

```sql
CREATE TABLE Funcionario (
    codFuncionario INTEGER PRIMARY KEY,
    nome VARCHAR(80),
    cpf CHAR(14),
    sexo CHAR(1),
    estadoCivil VARCHAR(30),
    codCargo INTEGER,
    FOREIGN KEY (codCargo)
        REFERENCES Cargo(codCargo)
);
```

## Conjuge

```sql
CREATE TABLE Conjuge (
    codConjuge INTEGER PRIMARY KEY,
    nome VARCHAR(80),
    telefone VARCHAR(20),
    codFuncionario INTEGER,
    FOREIGN KEY (codFuncionario)
        REFERENCES Funcionario(codFuncionario)
);
```

## Projeto

```sql
CREATE TABLE Projeto (
    codProjeto INTEGER PRIMARY KEY,
    nome VARCHAR(80),
    valor DECIMAL(10,2),
    dataInicio DATE,
    tempPrev INTEGER,
    porcent FLOAT
);
```

## Alocado

```sql
CREATE TABLE Alocado (
    codFuncionario INTEGER,
    codProjeto INTEGER,

    PRIMARY KEY (
        codFuncionario,
        codProjeto
    ),

    FOREIGN KEY (codFuncionario)
        REFERENCES Funcionario(codFuncionario),

    FOREIGN KEY (codProjeto)
        REFERENCES Projeto(codProjeto)
);
```

---

# 20. 🔍 Consultando os relacionamentos

Depois de criar as tabelas, podemos verificar suas estruturas:

```sql
SHOW TABLES;
```

```sql
DESC Cargo;
```

```sql
DESC Funcionario;
```

```sql
DESC Conjuge;
```

```sql
DESC Projeto;
```

```sql
DESC Alocado;
```

---

# 21. 🔎 Consultando funcionários e cargos

```sql
SELECT
    f.nome AS Funcionario,
    c.titulo AS Cargo,
    c.salario AS Salario
FROM Funcionario f
INNER JOIN Cargo c
    ON f.codCargo = c.codCargo;
```

---

# 22. 🔎 Consultando funcionários e cônjuges

```sql
SELECT
    f.nome AS Funcionario,
    c.nome AS Conjuge,
    c.telefone
FROM Funcionario f
INNER JOIN Conjuge c
    ON f.codFuncionario = c.codFuncionario;
```

Para também apresentar funcionários sem cônjuge cadastrado:

```sql
SELECT
    f.nome AS Funcionario,
    c.nome AS Conjuge,
    c.telefone
FROM Funcionario f
LEFT JOIN Conjuge c
    ON f.codFuncionario = c.codFuncionario;
```

---

# 23. 🔎 Consultando funcionários e projetos

```sql
SELECT
    f.nome AS Funcionario,
    p.nome AS Projeto
FROM Funcionario f
INNER JOIN Alocado a
    ON f.codFuncionario = a.codFuncionario
INNER JOIN Projeto p
    ON a.codProjeto = p.codProjeto;
```

---

# 24. 💰 Calculando o bônus

Considerando a porcentagem de participação aplicada ao valor do projeto:

```sql
SELECT
    f.nome AS Funcionario,
    p.nome AS Projeto,
    p.valor AS ValorProjeto,
    p.porcent AS Percentual,
    p.valor * (p.porcent / 100) AS Bonus
FROM Funcionario f
INNER JOIN Alocado a
    ON f.codFuncionario = a.codFuncionario
INNER JOIN Projeto p
    ON a.codProjeto = p.codProjeto;
```

---

# 25. 📌 Pontos principais da aula

### FN2

```text
Evita dependência parcial.
```

### FN3

```text
Evita dependência transitiva.
```

### Chave composta

```text
Mais de um campo participa da identificação
única do registro.
```

### Relacionamento N:N

```text
Funcionario
     ↕
  Alocado
     ↕
Projeto
```

### Normalização

```text
Reduz repetição
      ↓
Melhora organização
      ↓
Facilita manutenção
      ↓
Aumenta integridade dos dados
```

---

# 📝 Exercícios

## Exercício 01

Explique com suas palavras o que é a Segunda Forma Normal.

---

## Exercício 02

O que caracteriza uma dependência parcial?

---

## Exercício 03

Explique o que é dependência transitiva.

---

## Exercício 04

Por que `Cargo` foi separado de `Funcionario`?

---

## Exercício 05

Qual é a finalidade da tabela `Alocado`?

---

## Exercício 06

Qual é a chave primária da tabela `Alocado`?

---

## Exercício 07

Crie uma consulta que apresente:

```text
Funcionário
Cargo
Salário
```

---

## Exercício 08

Crie uma consulta que apresente:

```text
Funcionário
Projeto
Valor do projeto
Percentual
```

---

## Exercício 09

Crie uma consulta que calcule o bônus de participação.

---

## Exercício 10

Crie uma consulta utilizando `LEFT JOIN` para apresentar todos os funcionários e, quando existir, o respectivo cônjuge.

---

# ✅ Checklist da Aula 05

- [ ] Entender FN2.
- [ ] Entender dependência parcial.
- [ ] Entender FN3.
- [ ] Entender dependência transitiva.
- [ ] Identificar chave primária.
- [ ] Identificar chave estrangeira.
- [ ] Identificar chave composta.
- [ ] Entender relacionamento 1:N.
- [ ] Entender relacionamento 1:1.
- [ ] Entender relacionamento N:N.
- [ ] Criar tabela associativa.
- [ ] Utilizar `INNER JOIN`.
- [ ] Utilizar `LEFT JOIN`.
- [ ] Relacionar funcionário e cargo.
- [ ] Relacionar funcionário e cônjuge.
- [ ] Relacionar funcionário e projeto.
- [ ] Calcular bônus de participação.

---

# 📚 Resumo

Nesta aula, utilizamos um cenário empresarial para aprofundar os conceitos de **normalização**.

A FN2 trabalha principalmente com a eliminação de dependências parciais, enquanto a FN3 trata das dependências transitivas.

O modelo também apresenta uma situação importante em bancos relacionais: o relacionamento **muitos-para-muitos** entre funcionários e projetos.

Para representar esse relacionamento foi utilizada a tabela associativa `Alocado`, identificada pela combinação:

```text
codFuncionario + codProjeto
```

Dessa forma, o modelo fica estruturado para representar os funcionários, seus cargos, seus cônjuges e os projetos nos quais participam.

---

**Curso de Banco de Dados — MySQL**  
**Aula 05 — FN2, FN3 e Relacionamentos**
