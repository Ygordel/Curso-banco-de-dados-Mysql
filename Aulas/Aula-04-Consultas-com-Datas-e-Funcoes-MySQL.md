# 🗄️ Aula 04 — Datas, Horários e Funções no MySQL

**Curso:** Banco de Dados — MySQL  
**Aula:** 04  
**Tema:** Manipulação de datas, horários, cálculos temporais e funções numéricas

---

## 🎯 Objetivos

Nesta aula vamos trabalhar com recursos do MySQL para:

- criar um banco de dados voltado a um cenário de atendimento hospitalar;
- trabalhar com os tipos `DATE` e `DATETIME`;
- consultar data e hora do sistema;
- extrair dia, mês e ano de uma data;
- alterar a forma de apresentação das datas;
- realizar operações matemáticas com datas;
- calcular diferenças entre datas;
- estimar idade a partir da data de nascimento;
- utilizar `ROUND`, `FORMAT` e `TRUNCATE`;
- praticar consultas utilizando os dados criados durante o laboratório.

---

# 1. 🏥 Estudo de caso

O laboratório utiliza um cenário de **atendimento hospitalar**.

O sistema deverá registrar informações sobre:

### Médicos

- nome;
- especialidade.

### Pacientes

- nome;
- sexo;
- data de nascimento.

### Pessoa de contato

- nome;
- telefone;
- paciente relacionado.

O cadastro de contato é opcional para o paciente.

### Consultas

Cada atendimento deverá registrar:

- médico responsável;
- paciente atendido;
- diagnóstico;
- data e hora da consulta.

Um médico pode atender o mesmo paciente várias vezes e um paciente pode passar por diferentes médicos ao longo do tempo.

---

# 2. 🗃️ Criando o banco de dados

Primeiro vamos verificar os bancos existentes:

```sql
SHOW DATABASES;
```

Para o laboratório, vamos criar o banco `aula4`.

> ⚠️ O comando `DROP DATABASE` apaga o banco e seus dados. Utilize somente em ambiente de laboratório quando tiver certeza de que não existem informações importantes.

```sql
DROP DATABASE IF EXISTS aula4;

CREATE DATABASE aula4;

USE aula4;
```

---

# 3. 👨‍⚕️ Tabela de médicos

A tabela `medico` armazenará os profissionais responsáveis pelos atendimentos.

```sql
CREATE TABLE medico (
    codMedico INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    espec VARCHAR(60) NOT NULL
);
```

### Estrutura

| Campo | Tipo | Função |
|---|---|---|
| `codMedico` | `INTEGER` | Identificador do médico |
| `nome` | `VARCHAR(60)` | Nome |
| `espec` | `VARCHAR(60)` | Especialidade |

---

# 4. 🧑 Tabela de pacientes

Para o nascimento, utilizaremos o tipo `DATE`.

O MySQL trabalha internamente com a representação:

```text
AAAA-MM-DD
```

Exemplo:

```text
1990-04-24
```

Criação da tabela:

```sql
CREATE TABLE paciente (
    codPaciente INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    sexo ENUM('f', 'm') NOT NULL,
    dataNasc DATE NOT NULL
);
```

---

# 5. 📞 Tabela de contato

Cada paciente poderá possuir uma pessoa de contato.

Como `codPac` possui `UNIQUE`, o modelo permite somente um contato associado a cada paciente.

```sql
CREATE TABLE pessoaContato (
    codContato INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    telefone VARCHAR(15) NOT NULL,
    codPac INTEGER NOT NULL UNIQUE,
    FOREIGN KEY (codPac)
        REFERENCES paciente (codPaciente)
);
```

---

# 6. 🩺 Tabela de consultas

Para registrar **data e hora** utilizaremos `DATETIME`.

Exemplo:

```text
2022-03-14 12:00:00
```

Criação da tabela:

```sql
CREATE TABLE consulta (
    codConsulta INTEGER PRIMARY KEY AUTO_INCREMENT,
    cod_med INTEGER NOT NULL,
    cod_pac INTEGER NOT NULL,
    diagnostico VARCHAR(200) NOT NULL,
    dataHora DATETIME NOT NULL,
    FOREIGN KEY (cod_med)
        REFERENCES medico (codMedico),
    FOREIGN KEY (cod_pac)
        REFERENCES paciente (codPaciente)
);
```

---

# 7. 🔎 Conferindo as tabelas

```sql
SHOW TABLES;
```

Visualizando as estruturas:

```sql
DESC medico;
DESC paciente;
DESC pessoaContato;
DESC consulta;
```

---

# 8. 👨‍⚕️ Inserindo médicos

```sql
INSERT INTO medico VALUES
(NULL, 'Bia', 'Ortopedia'),
(NULL, 'Leo', 'Cardiologia'),
(NULL, 'Nat', 'Pediatria'),
(NULL, 'Ana', 'Clinica Médica');
```

Conferindo:

```sql
SELECT * FROM medico;
```

---

# 9. 🧑 Inserindo pacientes

```sql
INSERT INTO paciente (nome, dataNasc, sexo) VALUES
('Beto', '1965-06-28', 'm'),
('Rafa', '1978-03-05', 'f'),
('Hugo', '1949-08-17', 'm'),
('Duda', '1987-10-02', 'f'),
('Rita', '2012-03-12', 'f'),
('Mila', '1990-04-24', 'f'),
('José', '1982-11-30', 'm');
```

Consulta:

```sql
SELECT * FROM paciente;
```

---

# 10. 📞 Inserindo contatos

```sql
INSERT INTO pessoaContato VALUES
(NULL, 'Maria', '4444-3333', 4),
(NULL, 'Pedro', '9999-3333', 3),
(NULL, 'Paulo', '8888-3333', 6);
```

Conferindo:

```sql
SELECT * FROM pessoaContato;
```

---

# 11. 🩺 Inserindo consultas

```sql
INSERT INTO consulta
    (codConsulta, cod_med, cod_pac, diagnostico, dataHora)
VALUES
    (NULL, 4, 7, 'Dengue',      '2021-12-22 10:00'),
    (NULL, 1, 2, 'Fratura',     '2021-12-26 08:30'),
    (NULL, 3, 5, 'Catapora',    '2021-12-29 17:00'),
    (NULL, 4, 6, 'Virose',      '2022-01-03 14:00'),
    (NULL, 4, 4, 'Infeccao',    '2022-01-11 16:00'),
    (NULL, 1, 3, 'Entorse',     '2022-01-24 13:00'),
    (NULL, 2, 1, 'Hipertensao', '2022-01-30 11:20'),
    (NULL, 4, 2, 'Zika',        '2022-02-04 17:20'),
    (NULL, 1, 1, 'Fratura',     '2022-02-09 09:00'),
    (NULL, 4, 3, 'Gripe',       '2022-02-15 15:30'),
    (NULL, 2, 7, 'Arritmia',    '2022-03-01 16:30'),
    (NULL, 1, 2, 'Luxacao',     '2022-03-08 13:00'),
    (NULL, 4, 1, 'Pneumonia',   '2022-03-14 12:00'),
    (NULL, 3, 5, 'Gripe',       '2022-03-22 16:00'),
    (NULL, 2, 6, 'Arritmia',    '2022-03-29 13:00');
```

Conferindo:

```sql
SELECT * FROM consulta;
```

---

# 12. 📅 Trabalhando com datas no MySQL

O tipo `DATE` é utilizado para armazenar somente a data.

Formato:

```text
YYYY-MM-DD
```

Exemplo:

```text
2022-06-18
```

Já o tipo `DATETIME` permite armazenar data e horário:

```text
YYYY-MM-DD HH:MM:SS
```

Exemplo:

```text
2022-06-18 14:30:00
```

---

# 13. 📆 Data atual

Para consultar a data atual:

```sql
SELECT CURRENT_DATE;
```

Outra função relacionada ao momento atual é:

```sql
SELECT NOW();
```

---

# 14. ⏰ Hora atual

Para consultar somente o horário atual:

```sql
SELECT CURRENT_TIME;
```

---

# 15. 🕐 Data e hora atuais

Podemos utilizar:

```sql
SELECT SYSDATE();
```

ou:

```sql
SELECT NOW();
```

Essas funções permitem trabalhar com o momento atual do sistema.

---

# 16. 🔍 Extraindo dia, mês e ano

O MySQL possui funções específicas para retirar partes de uma data.

### Dia

```sql
SELECT DAY('2002-09-17') AS Dia;
```

### Mês

```sql
SELECT MONTH('2002-09-17') AS Mes;
```

### Ano

```sql
SELECT YEAR('2002-09-17') AS Ano;
```

Também podemos realizar as três operações em uma consulta:

```sql
SELECT
    DAY('2002-09-17') AS Dia,
    MONTH('2002-09-17') AS Mes,
    YEAR('2002-09-17') AS Ano;
```

---

# 17. 🎂 Extraindo o ano de nascimento

Podemos utilizar `YEAR()` diretamente em uma coluna.

```sql
SELECT
    nome,
    YEAR(dataNasc) AS AnoNascimento
FROM paciente;
```

---

# 18. 🎨 Formatando datas com DATE_FORMAT

O banco armazena a data em um formato próprio para processamento.

Para apresentação ao usuário, podemos utilizar `DATE_FORMAT()`.

### Formato brasileiro

```sql
SELECT DATE_FORMAT('2015-08-27', '%d/%m/%Y');
```

Resultado:

```text
27/08/2015
```

---

# 19. 🔤 Principais códigos do DATE_FORMAT

| Código | Significado |
|---|---|
| `%d` | Dia com dois dígitos |
| `%m` | Mês com dois dígitos |
| `%Y` | Ano com quatro dígitos |
| `%y` | Ano com dois dígitos |
| `%H` | Hora |
| `%i` | Minutos |
| `%W` | Nome do dia da semana |
| `%M` | Nome do mês |

---

# 20. 🧪 Exemplos de formatação

```sql
SELECT DATE_FORMAT('2015-08-27', '%d/%m/%y');
```

```sql
SELECT DATE_FORMAT('2015-08-27', '%d/%m/%Y');
```

```sql
SELECT DATE_FORMAT('2015-08-27', '%D %M %Y');
```

Podemos combinar informações:

```sql
SELECT DATE_FORMAT(
    NOW(),
    '%W, %D %M %Y - %H:%i'
);
```

---

# 21. 👤 Formatando nascimento dos pacientes

```sql
SELECT
    nome,
    DATE_FORMAT(dataNasc, '%d/%m/%Y') AS Nascimento
FROM paciente;
```

Essa abordagem é útil quando os dados precisam ser apresentados em formato mais amigável.

---

# 22. 🩺 Formatando data e hora da consulta

```sql
SELECT
    codConsulta,
    DATE_FORMAT(
        dataHora,
        '%d/%m/%Y - %H:%i'
    ) AS Atendimento
FROM consulta;
```

Exemplo de apresentação:

```text
03/01/2022 - 14:00
```

---

# 23. ➕ Somando períodos a uma data

A função `DATE_ADD()` permite adicionar um intervalo de tempo.

### Adicionando dias

```sql
SELECT DATE_ADD(
    '2022-03-13',
    INTERVAL 75 DAY
);
```

### Adicionando meses

```sql
SELECT DATE_ADD(
    '2022-03-13',
    INTERVAL 5 MONTH
);
```

---

# 24. ➖ Subtraindo períodos

Também podemos trabalhar com valores negativos:

```sql
SELECT DATE_ADD(
    '2022-03-13',
    INTERVAL -5 MONTH
);
```

Outra possibilidade é utilizar `DATE_SUB()`:

```sql
SELECT DATE_SUB(
    '2022-03-13',
    INTERVAL 5 MONTH
);
```

---

# 25. 📅 Calculando uma data de revisão

Podemos utilizar uma data existente na tabela.

Neste exemplo, serão adicionados 20 dias à data do atendimento:

```sql
SELECT
    codConsulta,
    cod_pac,
    dataHora,
    DATE_ADD(
        dataHora,
        INTERVAL 20 DAY
    ) AS DataRevisao
FROM consulta;
```

Também podemos formatar os resultados:

```sql
SELECT
    codConsulta,
    cod_pac,
    DATE_FORMAT(
        dataHora,
        '%d/%m/%Y - %H:%i'
    ) AS Atendimento,
    DATE_FORMAT(
        DATE_ADD(dataHora, INTERVAL 20 DAY),
        '%d/%m/%Y - %H:%i'
    ) AS DataRevisao
FROM consulta;
```

---

# 26. 📏 Calculando diferenças entre datas

A função `DATEDIFF()` retorna a diferença entre duas datas.

```sql
SELECT
    DATEDIFF(
        '2022-06-10',
        '2022-04-16'
    ) AS DiasDecorridos;
```

Também podemos comparar uma data com o momento atual:

```sql
SELECT
    DATEDIFF(
        NOW(),
        '2002-04-16'
    ) AS DiasDecorridos;
```

---

# 27. 🎂 Estimando idade

Uma forma trabalhada no laboratório é dividir a quantidade de dias por aproximadamente 365,25 dias por ano:

```sql
SELECT
    DATEDIFF(
        NOW(),
        '2002-04-16'
    ) / 365.25 AS Idade;
```

Aplicando aos pacientes:

```sql
SELECT
    nome,
    dataNasc,
    DATEDIFF(NOW(), dataNasc) / 365.25 AS Idade
FROM paciente;
```

> 💡 O cálculo acima é uma aproximação. Nesta aula ele será utilizado para demonstrar operações com datas e diferenças temporais.

---

# 28. 🔢 ROUND

`ROUND()` arredonda um valor numérico.

```sql
SELECT ROUND(13654.7618, 3);
```

```sql
SELECT ROUND(13654.7618, 2);
```

```sql
SELECT ROUND(13654.7618, 1);
```

```sql
SELECT ROUND(13654.7618, 0);
```

O segundo argumento determina a quantidade de casas decimais.

---

# 29. 💰 FORMAT

`FORMAT()` permite formatar valores numéricos.

```sql
SELECT FORMAT(13654.7618, 2);
```

Também podemos especificar uma localidade:

```sql
SELECT FORMAT(
    13654.7618,
    2,
    'pt_BR'
);
```

Exemplo com localidade alemã:

```sql
SELECT FORMAT(
    13654.7618,
    2,
    'de_DE'
);
```

---

# 30. ✂️ TRUNCATE

`TRUNCATE()` remove as casas decimais excedentes sem realizar arredondamento.

```sql
SELECT TRUNCATE(13654.7618, 2);
```

```sql
SELECT TRUNCATE(13654.7618, 1);
```

```sql
SELECT TRUNCATE(13654.7618, 0);
```

---

# 31. 🎂 Calculando a idade dos pacientes

Podemos combinar diferentes funções:

```sql
SELECT
    nome,
    DATE_FORMAT(
        dataNasc,
        '%d/%m/%Y'
    ) AS Nascimento,
    TRUNCATE(
        DATEDIFF(NOW(), dataNasc) / 365.25,
        0
    ) AS Idade
FROM paciente;
```

Neste exemplo são utilizadas três operações:

```text
DATE_FORMAT()
      ↓
apresentação da data

DATEDIFF()
      ↓
diferença entre datas

TRUNCATE()
      ↓
eliminação das casas decimais
```

---

# 32. 🧠 Resumo das funções

| Função | Utilização |
|---|---|
| `CURRENT_DATE` | Data atual |
| `CURRENT_TIME` | Hora atual |
| `NOW()` | Data e hora atuais |
| `SYSDATE()` | Data e hora do sistema |
| `DAY()` | Extrai o dia |
| `MONTH()` | Extrai o mês |
| `YEAR()` | Extrai o ano |
| `DATE_FORMAT()` | Formata datas |
| `DATE_ADD()` | Adiciona intervalos |
| `DATE_SUB()` | Subtrai intervalos |
| `DATEDIFF()` | Calcula diferença entre datas |
| `ROUND()` | Arredonda números |
| `FORMAT()` | Formata números |
| `TRUNCATE()` | Trunca casas decimais |

---

# 33. 🧪 Laboratório completo

Execute os comandos abaixo em um ambiente de testes do MySQL.

```sql
DROP DATABASE IF EXISTS aula4;

CREATE DATABASE aula4;

USE aula4;

CREATE TABLE medico (
    codMedico INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    espec VARCHAR(60) NOT NULL
);

CREATE TABLE paciente (
    codPaciente INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    sexo ENUM('f', 'm') NOT NULL,
    dataNasc DATE NOT NULL
);

CREATE TABLE pessoaContato (
    codContato INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(60) NOT NULL,
    telefone VARCHAR(15) NOT NULL,
    codPac INTEGER NOT NULL UNIQUE,
    FOREIGN KEY (codPac)
        REFERENCES paciente (codPaciente)
);

CREATE TABLE consulta (
    codConsulta INTEGER PRIMARY KEY AUTO_INCREMENT,
    cod_med INTEGER NOT NULL,
    cod_pac INTEGER NOT NULL,
    diagnostico VARCHAR(200) NOT NULL,
    dataHora DATETIME NOT NULL,
    FOREIGN KEY (cod_med)
        REFERENCES medico (codMedico),
    FOREIGN KEY (cod_pac)
        REFERENCES paciente (codPaciente)
);

INSERT INTO medico VALUES
(NULL, 'Bia', 'Ortopedia'),
(NULL, 'Leo', 'Cardiologia'),
(NULL, 'Nat', 'Pediatria'),
(NULL, 'Ana', 'Clinica Médica');

INSERT INTO paciente (nome, dataNasc, sexo) VALUES
('Beto', '1965-06-28', 'm'),
('Rafa', '1978-03-05', 'f'),
('Hugo', '1949-08-17', 'm'),
('Duda', '1987-10-02', 'f'),
('Rita', '2012-03-12', 'f'),
('Mila', '1990-04-24', 'f'),
('José', '1982-11-30', 'm');

INSERT INTO pessoaContato VALUES
(NULL, 'Maria', '4444-3333', 4),
(NULL, 'Pedro', '9999-3333', 3),
(NULL, 'Paulo', '8888-3333', 6);

INSERT INTO consulta
    (codConsulta, cod_med, cod_pac, diagnostico, dataHora)
VALUES
    (NULL, 4, 7, 'Dengue',      '2021-12-22 10:00'),
    (NULL, 1, 2, 'Fratura',     '2021-12-26 08:30'),
    (NULL, 3, 5, 'Catapora',    '2021-12-29 17:00'),
    (NULL, 4, 6, 'Virose',      '2022-01-03 14:00'),
    (NULL, 4, 4, 'Infeccao',    '2022-01-11 16:00'),
    (NULL, 1, 3, 'Entorse',     '2022-01-24 13:00'),
    (NULL, 2, 1, 'Hipertensao', '2022-01-30 11:20'),
    (NULL, 4, 2, 'Zika',        '2022-02-04 17:20'),
    (NULL, 1, 1, 'Fratura',     '2022-02-09 09:00'),
    (NULL, 4, 3, 'Gripe',       '2022-02-15 15:30'),
    (NULL, 2, 7, 'Arritmia',    '2022-03-01 16:30'),
    (NULL, 1, 2, 'Luxacao',     '2022-03-08 13:00'),
    (NULL, 4, 1, 'Pneumonia',   '2022-03-14 12:00'),
    (NULL, 3, 5, 'Gripe',       '2022-03-22 16:00'),
    (NULL, 2, 6, 'Arritmia',    '2022-03-29 13:00');

SHOW TABLES;

DESC medico;
DESC paciente;
DESC pessoaContato;
DESC consulta;

SELECT CURRENT_DATE;

SELECT CURRENT_TIME;

SELECT NOW();

SELECT SYSDATE();

SELECT
    DAY('2002-09-17') AS Dia,
    MONTH('2002-09-17') AS Mes,
    YEAR('2002-09-17') AS Ano;

SELECT
    nome,
    YEAR(dataNasc) AS AnoNascimento
FROM paciente;

SELECT
    nome,
    DATE_FORMAT(dataNasc, '%d/%m/%Y') AS Nascimento
FROM paciente;

SELECT
    codConsulta,
    DATE_FORMAT(
        dataHora,
        '%d/%m/%Y - %H:%i'
    ) AS Atendimento
FROM consulta;

SELECT
    codConsulta,
    cod_pac,
    DATE_FORMAT(
        dataHora,
        '%d/%m/%Y - %H:%i'
    ) AS Atendimento,
    DATE_FORMAT(
        DATE_ADD(dataHora, INTERVAL 20 DAY),
        '%d/%m/%Y - %H:%i'
    ) AS DataRevisao
FROM consulta;

SELECT
    nome,
    dataNasc,
    TRUNCATE(
        DATEDIFF(NOW(), dataNasc) / 365.25,
        0
    ) AS Idade
FROM paciente;
```

---

# 34. 📝 Exercícios

### Exercício 01

Exiba o nome e o ano de nascimento de todos os pacientes.

---

### Exercício 02

Exiba o nome dos pacientes e a data de nascimento no formato:

```text
DD/MM/AAAA
```

---

### Exercício 03

Exiba o código da consulta e a data/hora do atendimento no formato:

```text
DD/MM/AAAA - HH:MM
```

---

### Exercício 04

Calcule uma data de revisão acrescentando **20 dias** a cada atendimento.

---

### Exercício 05

Calcule a quantidade de dias decorridos desde a data de nascimento de cada paciente.

---

### Exercício 06

Calcule uma idade aproximada para cada paciente.

---

### Exercício 07

Utilize `ROUND()` para arredondar:

```text
13654.7618
```

para duas casas decimais.

---

### Exercício 08

Utilize `FORMAT()` para apresentar o número:

```text
13654.7618
```

com duas casas decimais.

---

### Exercício 09

Utilize `TRUNCATE()` para apresentar o mesmo número com duas casas decimais, sem arredondamento.

---

### Exercício 10

Crie uma consulta que mostre:

```text
Nome do paciente
Data de nascimento
Idade aproximada
```

Utilizando `DATE_FORMAT`, `DATEDIFF` e `TRUNCATE`.

---

# 35. ✅ Checklist

- [ ] Criar o banco `aula4`.
- [ ] Criar a tabela `medico`.
- [ ] Criar a tabela `paciente`.
- [ ] Criar a tabela `pessoaContato`.
- [ ] Criar a tabela `consulta`.
- [ ] Entender `DATE`.
- [ ] Entender `DATETIME`.
- [ ] Consultar a data atual.
- [ ] Consultar a hora atual.
- [ ] Consultar data e hora atuais.
- [ ] Extrair dia, mês e ano.
- [ ] Formatar datas.
- [ ] Adicionar dias e meses.
- [ ] Subtrair períodos.
- [ ] Calcular diferenças entre datas.
- [ ] Estimar idade.
- [ ] Utilizar `ROUND`.
- [ ] Utilizar `FORMAT`.
- [ ] Utilizar `TRUNCATE`.

---

# 📌 Resumo da aula

Nesta aula, construímos um pequeno banco de dados para representar um ambiente hospitalar e utilizamos esse cenário para estudar o tratamento de **datas, horários e valores numéricos no MySQL**.

O fluxo principal foi:

```text
Banco de dados
      ↓
Tabelas
      ↓
DATE / DATETIME
      ↓
Consultas
      ↓
Extração de partes da data
      ↓
Formatação
      ↓
Cálculos temporais
      ↓
Funções numéricas
```

As funções mais importantes praticadas foram:

```text
CURRENT_DATE
CURRENT_TIME
NOW()
SYSDATE()
DAY()
MONTH()
YEAR()
DATE_FORMAT()
DATE_ADD()
DATE_SUB()
DATEDIFF()
ROUND()
FORMAT()
TRUNCATE()
```

---

**Curso de Banco de Dados — MySQL**  
**Aula 04 — Datas, Horários e Funções**
