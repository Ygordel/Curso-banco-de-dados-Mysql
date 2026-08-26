# 📚 Aula 02 — Modelagem de Dados e Ferramentas do Curso

**Curso:** Banco de Dados com MySQL  
**Aula:** 02  
**Tema:** Modelagem de dados, entidades, atributos, relacionamentos e ferramentas  
**Professor:** Ygor Silva  
**Tecnologia principal:** MySQL  

---

## 🎯 Objetivos da aula

Nesta aula vamos começar a transformar um problema real em uma estrutura de banco de dados.

Ao final da aula, o aluno deverá compreender:

- o que é modelagem de dados;
- o conceito de entidade;
- o conceito de atributo;
- o conceito de relacionamento;
- a função do Modelo Entidade-Relacionamento (MER);
- a relação entre modelo e tabelas do banco;
- o conceito de chave primária;
- o conceito de `NOT NULL`;
- como criar uma tabela de clientes;
- como inserir e consultar registros;
- como utilizar filtros com `WHERE`;
- como utilizar `AND` e `OR`;
- como realizar pesquisas textuais com `LIKE`.

A aula parte da análise do escopo do projeto para definir as entidades, atributos e relacionamentos que posteriormente serão transformados em estruturas do banco de dados.
---

# 🧰 1. Ferramentas utilizadas no curso

Para acompanhar as aulas práticas, serão utilizados diferentes programas, cada um com uma finalidade específica.

## 🗺️ 1.1 brModelo

O **brModelo** será utilizado para trabalhar a **modelagem de dados** e representar graficamente as estruturas do banco.

A ferramenta permite visualizar a organização das entidades e seus atributos antes da implementação no MySQL.

### Arquivo disponibilizado

```text
brModelo30.zip
```

### Utilização no curso

O brModelo será utilizado principalmente para:

- criar modelos de dados;
- representar entidades;
- definir atributos;
- visualizar relacionamentos;
- organizar a estrutura lógica do banco;
- auxiliar na construção das tabelas.

> 💡 **Ideia principal:** antes de criar as tabelas no MySQL, vamos pensar e desenhar a estrutura do banco.

---

## 📝 1.2 Notepad++

O **Notepad++** será utilizado como editor de textos e arquivos de comandos.

### Arquivo disponibilizado

```text
npp.7.8.8.Installer.x64.exe
```

### Utilização no curso

Pode ser utilizado para:

- escrever scripts SQL;
- editar arquivos `.sql`;
- organizar comandos;
- salvar exercícios;
- revisar comandos antes de executá-los;
- manter uma cópia dos scripts das aulas.

Exemplo de arquivo:

```text
Aula-02-Banco-de-Dados.sql
```

---

## 🗄️ 1.3 MySQL

O **MySQL** será o principal sistema de gerenciamento de banco de dados utilizado no curso.

### Instalador disponibilizado

```text
mysql-installer-community-8.0.21.0.msi
```

Com o MySQL será possível:

- criar bancos de dados;
- criar tabelas;
- inserir registros;
- alterar dados;
- excluir dados;
- realizar consultas;
- utilizar filtros;
- trabalhar com chaves;
- executar comandos SQL.

---

# 🧩 2. Como as ferramentas se relacionam

Durante o curso, podemos pensar no processo da seguinte forma:

```text
PROBLEMA / REGRA DE NEGÓCIO
          ↓
     MODELAGEM
          ↓
       brModelo
          ↓
 ENTIDADES E ATRIBUTOS
          ↓
       SQL / SCRIPT
          ↓
       MySQL
          ↓
     BANCO DE DADOS
          ↓
       CONSULTAS
```

O **brModelo** ajuda a representar a estrutura.

O **Notepad++** pode ser utilizado para escrever e organizar os scripts.

O **MySQL** executa os comandos e mantém os dados.

---

# 🧠 3. Modelagem de dados

A modelagem de dados começa pela análise do problema que será resolvido.

O objetivo é identificar:

- entidades;
- atributos;
- relacionamentos;
- regras necessárias para representar o negócio.

O material da aula destaca que os elementos identificados no escopo devem ser representados graficamente para auxiliar na construção das tabelas, campos e relacionamentos. 

---

# 🏢 4. Entidades

Uma **entidade** representa algo relevante para o sistema e sobre o qual precisamos armazenar informações.

Exemplos:

```text
Cliente
Produto
Funcionário
Fornecedor
Pedido
Pagamento
```

No estudo de caso da aula, a entidade principal é:

```text
Cliente
```

---

# 🏷️ 5. Atributos

Os atributos representam as informações que desejamos armazenar sobre uma entidade.

Para a entidade `Cliente`, o estudo de caso apresenta:

```text
nome
email
sexo
estadoCivil
renda
telefone
endereco
```

Esses dados são utilizados para representar as informações relevantes do cliente. fileciteturn18file0L20-L34

---

# 🔗 6. Relacionamentos

Os relacionamentos representam as ligações entre entidades.

Exemplo:

```text
Cliente ───── realiza ───── Pedido
```

Em um banco de dados completo, um cliente pode estar relacionado a diversos pedidos.

Nesta aula, o foco inicial será a construção da entidade `Cliente` e de sua tabela.

---

# 📐 7. MER — Modelo Entidade-Relacionamento

O **MER (Modelo Entidade-Relacionamento)** é uma representação gráfica dos objetos que serão utilizados na construção do banco.

Ele auxilia na visualização de:

```text
ENTIDADES
   ↓
ATRIBUTOS
   ↓
RELACIONAMENTOS
   ↓
TABELAS
```

O material da Aula 02 utiliza o MER para representar a entidade `Cliente` e seus atributos. 

---

# 👤 8. Estudo de caso — Cliente

Vamos considerar uma empresa que deseja armazenar informações de seus clientes.

Os dados necessários são:

| Campo | Informação |
|---|---|
| `nome` | Nome do cliente |
| `email` | E-mail |
| `sexo` | Sexo |
| `estadoCivil` | Estado civil |
| `renda` | Renda |
| `telefone` | Telefone |
| `endereco` | Endereço |

---

# 🔑 9. Chave Primária

A **PRIMARY KEY** identifica de forma única cada registro da tabela.

No nosso exemplo:

```text
codigoCliente
```

Será utilizado como chave primária.

Características:

- identifica o registro;
- não deve possuir valores duplicados;
- não aceita valor nulo.

O material utiliza `codigoCliente INT PRIMARY KEY` na tabela `cliente`. 

---

# 🚫 10. NOT NULL

O `NOT NULL` determina que o campo deve receber um valor.

Exemplo:

```sql
nome VARCHAR(50) NOT NULL
```

Nesse caso, o campo `nome` é obrigatório.

---

# 🗃️ 11. Criando a tabela Cliente

Primeiro selecionamos o banco:

```sql
USE aula1;
```

Depois criamos a tabela:

```sql
CREATE TABLE cliente(
    codigoCliente INT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    email VARCHAR(60) NOT NULL,
    sexo CHAR NOT NULL,
    estadoCivil VARCHAR(40) NOT NULL,
    renda FLOAT NOT NULL,
    endereco VARCHAR(200) NOT NULL,
    telefone VARCHAR(60) NOT NULL
);
```

A estrutura acima segue o exemplo utilizado no material da Aula 02.

---

# 🔎 12. Verificando a tabela

### Listar tabelas

```sql
SHOW TABLES;
```

### Ver a estrutura

```sql
DESC cliente;
```

---

# ➕ 13. Inserindo clientes

Exemplos:

```sql
INSERT INTO cliente
VALUES(
    101,
    'Leo',
    'leo@uol.com',
    'm',
    'Solteiro',
    4200,
    'Rua a, Cidade q, RJ',
    '3333-4444 / 5555-6677'
);
```

```sql
INSERT INTO cliente
VALUES(
    102,
    'Mel',
    'mel@uol.com',
    'f',
    'Solteiro',
    3500,
    'Rua s, Cidade w, RJ',
    '2233-4444 / 5555-6688'
);
```

```sql
INSERT INTO cliente
VALUES(
    103,
    'Bia',
    'bia@aol.com',
    'f',
    'Solteiro',
    6500,
    'Rua d, Cidade e, SP',
    '4433-4444 / 5555-6699'
);
```

O material também apresenta outros registros para complementar a prática. 

---

# 🔍 14. Consultando os clientes

```sql
SELECT * FROM cliente;
```

---

# ✏️ 15. Alterando registros

Podemos alterar um registro com base em uma condição.

### Exemplo pelo nome

```sql
UPDATE cliente
SET sexo = 'm'
WHERE nome = 'Edu';
```

### Exemplo utilizando a chave primária

```sql
UPDATE cliente
SET sexo = 'm'
WHERE codigoCliente = 105;
```

O uso da chave primária é uma forma mais segura de identificar exatamente o registro que será alterado. 

---

# 🗑️ 16. Excluindo registros

Para excluir um registro específico:

```sql
DELETE FROM cliente
WHERE codigoCliente = 108;
```

Depois podemos conferir:

```sql
SELECT * FROM cliente;
```

fileciteturn18file0L94-L97

---

# 🎯 17. Consultas com WHERE

O `WHERE` permite estabelecer condições para selecionar registros.

### Clientes com renda superior a 6000

```sql
SELECT nome, sexo, renda
FROM cliente
WHERE renda > 6000;
```

### Clientes com renda entre 4000 e 6000

```sql
SELECT nome, sexo, renda
FROM cliente
WHERE renda >= 4000
AND renda <= 6000;
```

---

# 🔀 18. Utilizando AND e OR

### AND

Todas as condições precisam ser verdadeiras.

```sql
SELECT nome, sexo, estadoCivil, renda
FROM cliente
WHERE sexo = 'f'
AND estadoCivil = 'Solteiro';
```

### OR

Pelo menos uma das condições deve ser atendida.

```sql
SELECT nome, sexo, estadoCivil, renda
FROM cliente
WHERE sexo = 'm'
OR estadoCivil = 'Casado';
```

Esses operadores são utilizados no material para realizar consultas condicionais sobre os clientes. 
---

# 🔤 19. Pesquisa textual com LIKE

O operador `LIKE` permite realizar pesquisas utilizando padrões de texto.

### Nome começando com L

```sql
SELECT nome, email
FROM cliente
WHERE nome LIKE 'L%';
```

### Nome terminando com "a"

```sql
SELECT nome, email
FROM cliente
WHERE nome LIKE '%a';
```

### E-mails contendo `@aol`

```sql
SELECT nome, email
FROM cliente
WHERE email LIKE '%@aol%';
```

### Endereços contendo RJ

```sql
SELECT nome, endereco
FROM cliente
WHERE endereco LIKE '%RJ%';
```

O material apresenta `LIKE` como recurso para pesquisa textual e demonstra diferentes posições do caractere `%`. 

---

# 📊 20. Ordenando resultados

Podemos combinar `WHERE` e `ORDER BY`.

Exemplo:

```sql
SELECT nome, sexo, renda
FROM cliente
ORDER BY renda DESC;
```

Para pesquisar somente clientes do sexo feminino e ordenar pelo nome:

```sql
SELECT nome, sexo, renda
FROM cliente
WHERE sexo = 'f'
ORDER BY nome;
```

---

# 🧪 21. Laboratório da Aula 02

Execute os comandos na sequência:

```sql
USE aula1;

CREATE TABLE cliente(
    codigoCliente INT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    email VARCHAR(60) NOT NULL,
    sexo CHAR NOT NULL,
    estadoCivil VARCHAR(40) NOT NULL,
    renda FLOAT NOT NULL,
    endereco VARCHAR(200) NOT NULL,
    telefone VARCHAR(60) NOT NULL
);

INSERT INTO cliente VALUES
(101, 'Leo', 'leo@uol.com', 'm', 'Solteiro', 4200,
'Rua a, Cidade q, RJ', '3333-4444 / 5555-6677');

INSERT INTO cliente VALUES
(102, 'Mel', 'mel@uol.com', 'f', 'Solteiro', 3500,
'Rua s, Cidade w, RJ', '2233-4444 / 5555-6688');

INSERT INTO cliente VALUES
(103, 'Bia', 'bia@aol.com', 'f', 'Solteiro', 6500,
'Rua d, Cidade e, SP', '4433-4444 / 5555-6699');

SELECT * FROM cliente;

SELECT nome, sexo, renda
FROM cliente
ORDER BY renda DESC;

SELECT nome, sexo, renda
FROM cliente
WHERE sexo = 'f'
ORDER BY nome;

SELECT nome, sexo, renda
FROM cliente
WHERE sexo = 'f'
AND renda > 6000;

SELECT nome, email
FROM cliente
WHERE email LIKE '%@aol%';

SELECT nome, endereco
FROM cliente
WHERE endereco LIKE '%RJ%';
```

---

# 📝 22. Atividade prática

### Parte 1 — Modelagem

No **brModelo**:

1. Crie um novo modelo.
2. Crie a entidade `Cliente`.
3. Adicione os atributos:
   - `codigoCliente`
   - `nome`
   - `email`
   - `sexo`
   - `estadoCivil`
   - `renda`
   - `telefone`
   - `endereco`
4. Defina `codigoCliente` como identificador.
5. Organize visualmente o modelo.
6. Salve o arquivo do modelo.

### Parte 2 — MySQL

No MySQL:

1. Selecione o banco `aula1`.
2. Crie a tabela `cliente`.
3. Insira os registros.
4. Consulte os clientes.
5. Faça uma consulta por renda.
6. Faça uma consulta utilizando `AND`.
7. Faça uma consulta utilizando `OR`.
8. Faça uma pesquisa utilizando `LIKE`.
9. Altere um registro.
10. Exclua um registro utilizando a chave primária.

---


