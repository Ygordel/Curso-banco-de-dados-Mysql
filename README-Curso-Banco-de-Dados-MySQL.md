# 📚 Curso de Banco de Dados — MySQL

![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?logo=mysql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-Banco%20de%20Dados-blue)
![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-orange)
![GitHub](https://img.shields.io/badge/GitHub-Curso%20Pr%C3%A1tico-black?logo=github)

## 👨‍💻 Apresentação

Bem-vindo ao **Curso de Banco de Dados com foco em MySQL**.

Este repositório foi criado para organizar uma trilha de estudos **prática e progressiva**, começando pela preparação do ambiente e instalação do MySQL 8 e avançando para SQL, modelagem, administração, segurança, desempenho, backup e projetos práticos.

A proposta é transformar cada conteúdo em um laboratório documentado, permitindo acompanhar a evolução do aprendizado por meio de **comandos, exemplos, exercícios e evidências práticas**.

---

## 🎯 Objetivo

O objetivo deste curso é desenvolver conhecimentos para trabalhar com bancos de dados relacionais utilizando o **MySQL**.

Ao longo da trilha serão abordados desde os conceitos fundamentais até recursos utilizados na administração e desenvolvimento de bancos de dados.

### Ao final do curso, o aluno deverá ser capaz de:

- compreender os fundamentos de bancos de dados;
- instalar e configurar o MySQL;
- utilizar o MySQL Workbench e o terminal;
- criar e administrar bancos de dados;
- criar tabelas e relacionamentos;
- inserir, alterar e excluir dados;
- realizar consultas SQL;
- utilizar filtros, agrupamentos e ordenações;
- trabalhar com JOINs;
- compreender chaves primárias e estrangeiras;
- aplicar conceitos de modelagem e normalização;
- criar Views;
- desenvolver Stored Procedures;
- utilizar Functions;
- trabalhar com Triggers;
- criar e analisar índices;
- administrar usuários e privilégios;
- aplicar conceitos básicos de segurança;
- realizar backup e restauração;
- analisar desempenho;
- desenvolver um projeto final de banco de dados.

---

# 🧭 Metodologia

O curso será desenvolvido em formato de **laboratório prático**.

Cada etapa deverá apresentar:

```text
📚 Conceito
    ↓
⚙️ Configuração
    ↓
💻 Comandos
    ↓
🧪 Laboratório
    ↓
🔎 Validação
    ↓
📸 Evidências
    ↓
✅ Checklist
```

A documentação será organizada para facilitar tanto o estudo quanto a consulta posterior.

---

# 🗂️ Conteúdo programático

| Etapa | Conteúdo |
|---:|---|
| 01 | Instalação do MySQL 8 |
| 02 | Primeiros comandos e acesso ao MySQL |
| 03 | Bancos de dados e tabelas |
| 04 | Tipos de dados |
| 05 | DML — INSERT, UPDATE e DELETE |
| 06 | DQL — SELECT e consultas |
| 07 | Filtros, ordenação e agrupamento |
| 08 | JOINs |
| 09 | Chaves e relacionamentos |
| 10 | Modelagem de dados |
| 11 | Normalização |
| 12 | Views |
| 13 | Stored Procedures |
| 14 | Functions |
| 15 | Triggers |
| 16 | Índices e desempenho |
| 17 | Usuários e privilégios |
| 18 | Segurança |
| 19 | Backup e restauração |
| 20 | Administração e monitoramento |
| 21 | Projeto prático final |

---

# 🛠️ Tecnologias e ferramentas

Durante a trilha poderão ser utilizados:

- **MySQL 8**
- **MySQL Workbench**
- **SQL**
- **Windows**
- **CMD / PowerShell**
- **Git**
- **GitHub**

---

# 📁 Organização do repositório

```text
curso-banco-de-dados-mysql/
│
├── README.md
│
├── 01-Instalacao-MySQL-8/
│   └── README.md
│
├── 02-Primeiros-Comandos/
│   └── README.md
│
├── 03-Bancos-e-Tabelas/
│   └── README.md
│
├── 04-Tipos-de-Dados/
│   └── README.md
│
├── 05-DML/
│   └── README.md
│
├── 06-DQL/
│   └── README.md
│
├── 07-Filtros-e-Agrupamentos/
│   └── README.md
│
├── 08-JOINs/
│   └── README.md
│
├── 09-Chaves-e-Relacionamentos/
│   └── README.md
│
├── 10-Modelagem/
│   └── README.md
│
├── 11-Normalizacao/
│   └── README.md
│
├── 12-Views/
│   └── README.md
│
├── 13-Procedures/
│   └── README.md
│
├── 14-Functions/
│   └── README.md
│
├── 15-Triggers/
│   └── README.md
│
├── 16-Indices-e-Performance/
│   └── README.md
│
├── 17-Usuarios-e-Privilégios/
│   └── README.md
│
├── 18-Seguranca/
│   └── README.md
│
├── 19-Backup-e-Restore/
│   └── README.md
│
├── 20-Administracao/
│   └── README.md
│
└── 21-Projeto-Final/
    └── README.md
```

---

# 🚀 Primeira etapa

## Instalação do MySQL 8

A primeira etapa prepara o ambiente para os laboratórios seguintes.

Serão abordados:

- MySQL Installer;
- MySQL Server 8;
- MySQL Workbench;
- verificação de requisitos;
- configuração do servidor;
- configuração de rede;
- porta TCP 3306;
- método de autenticação;
- criação da conta `root`;
- configuração do serviço `MySQL80`;
- validação da instalação;
- primeiro acesso pelo terminal.

### Primeiro teste

Após a instalação:

```bash
mysql --version
```

Acesso ao servidor:

```bash
mysql -u root -p
```

Dentro do MySQL:

```sql
SELECT VERSION();
SELECT USER();
SHOW DATABASES;
```

---

# 🧪 Laboratórios

Cada módulo deverá possuir atividades práticas.

Exemplo:

```text
LABORATÓRIO 01
Instalação e validação do MySQL

LABORATÓRIO 02
Criação do primeiro banco

LABORATÓRIO 03
Criação de tabelas

LABORATÓRIO 04
Inserção de dados

LABORATÓRIO 05
Consultas SQL

...
```

---

# 📸 Evidências

Sempre que possível, os laboratórios deverão registrar evidências da execução.

Exemplos:

- terminal;
- MySQL Workbench;
- comandos SQL;
- resultados das consultas;
- estrutura das tabelas;
- diagramas;
- mensagens de sucesso ou validação.

> ⚠️ Senhas, tokens, credenciais reais e informações sensíveis não devem ser publicados no GitHub.

---

# 📈 Evolução do curso

A ideia é que este repositório evolua junto com os estudos.

```text
Fundamentos
     ↓
Instalação
     ↓
SQL
     ↓
Modelagem
     ↓
Relacionamentos
     ↓
Recursos avançados
     ↓
Administração
     ↓
Segurança
     ↓
Performance
     ↓
Projeto Final
```

---

# 🎓 Projeto final

Ao final da trilha será desenvolvido um banco de dados completo, contemplando:

- levantamento de requisitos;
- modelagem;
- criação das tabelas;
- definição das chaves;
- relacionamentos;
- inserção de dados;
- consultas;
- Views;
- Procedures;
- Functions;
- Triggers;
- índices;
- usuários e permissões;
- backup;
- documentação.

O projeto servirá como consolidação dos conhecimentos desenvolvidos durante o curso.

---

# 📌 Status

**Curso em desenvolvimento.**

Novas etapas serão adicionadas progressivamente ao repositório.

---

## 👨‍💻 Autor

**Ygor Silva**

Curso prático de Banco de Dados com foco em MySQL.

---

> 📚 **Estude. Pratique. Documente. Valide. Evolua.**
>
> Este repositório representa a construção prática do conhecimento em Banco de Dados e MySQL.
