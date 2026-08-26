# Curso de Banco de Dados — MySQL 8

## ETAPA 01 — Instalação do MySQL 8

### Objetivo

Preparar o ambiente de laboratório para o curso de Banco de Dados utilizando o MySQL 8, instalando o MySQL Server e o MySQL Workbench.

O material de referência apresenta a instalação por meio do MySQL Installer, incluindo seleção dos produtos, verificação de requisitos, configuração do servidor, rede, autenticação, conta administrativa, serviço do Windows e aplicação da configuração. 

---

## 1. Seleção dos produtos

Abra o **MySQL Installer** e selecione os componentes necessários para o laboratório.

Produtos utilizados:

- MySQL Server 8.0
- MySQL Workbench 8.0

A documentação de referência apresenta esses dois produtos na tela de seleção. fileciteturn16file0L2-L4

---

## 2. Verificação dos requisitos

Na etapa **Check Requirements**, verifique os componentes necessários para a instalação.

Caso seja solicitado o **Microsoft Visual C++ Redistributable**, execute a instalação do requisito e aguarde a conclusão.

Depois, retorne ao MySQL Installer e continue o processo.

A documentação de referência apresenta essa etapa e a instalação do requisito pelo próprio instalador. fileciteturn16file0L5-L7

---

## 3. Instalação

Confirme os produtos selecionados e execute a instalação.

A lista deverá conter:

```text
MySQL Server 8.0
MySQL Workbench 8.0
```

Aguarde a conclusão antes de avançar. fileciteturn16file0L9-L10

---

## 4. Configuração do servidor

### Alta disponibilidade

Para o laboratório inicial, utilize:

```text
Standalone MySQL Server / Classic MySQL Replication
```

O material de referência apresenta essa opção na tela **High Availability**. fileciteturn16file0L9-L10

---

## 5. Type and Networking

Utilize:

```text
Config Type: Development Computer
```

A porta padrão do MySQL será:

```text
3306/TCP
```

Mantenha a conectividade TCP/IP habilitada.

Se o laboratório precisar de acesso pela rede, avalie a abertura da porta no Firewall do Windows.

Essas opções aparecem na tela **Type and Networking** do material de referência. fileciteturn16file0L11-L11

---

## 6. Método de autenticação

Utilize, preferencialmente:

```text
Use Strong Password Encryption for Authentication
```

O material também apresenta a opção:

```text
Use Legacy Authentication Method
```

A opção Legacy deve ficar reservada para cenários de compatibilidade com aplicações antigas. fileciteturn16file0L11-L11

---

## 7. Conta administrativa

Na tela **Accounts and Roles**, configure a senha da conta:

```text
root
```

Utilize uma senha forte.

Exemplo de referência do laboratório:

```text
Usuário: root
Servidor: localhost
Porta: 3306
```

O material de referência apresenta a configuração da senha do usuário `root`. fileciteturn16file0L12-L12

> ⚠️ Nunca publique uma senha real no GitHub.

---

## 8. Windows Service

Configure o MySQL como serviço do Windows.

Nome sugerido:

```text
MySQL80
```

Após a instalação, a existência do serviço pode ser conferida com:

```text
services.msc
```

Procure por:

```text
MySQL80
```

A documentação de referência apresenta essa configuração na etapa **Windows Service**. fileciteturn16file0L12-L12

---

## 9. Aplicar configuração

Avance até **Apply Configuration**.

O instalador realizará operações como:

- criação do arquivo de configuração;
- atualização das regras do Firewall;
- configuração do serviço;
- inicialização do banco;
- aplicação das configurações de segurança;
- inicialização do servidor.

O material de referência apresenta essas operações concluídas com sucesso. fileciteturn16file0L13-L15

---

## 10. Finalização

Ao concluir a configuração, selecione:

```text
Finish
```

A instalação do MySQL Server e do MySQL Workbench estará concluída.

---

# ETAPA 02 — Validação da instalação

## 11. Verificar o serviço

No Windows:

```text
Win + R
```

Execute:

```text
services.msc
```

Localize:

```text
MySQL80
```

Verifique se o serviço está em execução.

---

## 12. Verificar a versão

Abra o CMD ou PowerShell:

```bash
mysql --version
```

Resultado esperado semelhante a:

```text
mysql  Ver 8.0.x for Win64
```

A versão exata depende da versão instalada no laboratório.

---

## 13. Acessar o MySQL

Execute:

```bash
mysql -u root -p
```

Informe a senha definida durante a instalação.

O acesso bem-sucedido apresentará:

```text
mysql>
```

---

## 14. Primeiros comandos

Dentro do MySQL:

```sql
SELECT VERSION();
```

```sql
SELECT USER();
```

```sql
SHOW DATABASES;
```

Para sair:

```sql
EXIT;
```

---

# LABORATÓRIO 01 — Validação

Execute:

```bash
mysql --version
```

Depois:

```bash
mysql -u root -p
```

No prompt do MySQL:

```sql
SELECT VERSION();
SELECT USER();
SHOW DATABASES();
```

### Registro

```text
Sistema operacional: ______________________

Versão do MySQL: __________________________

Usuário: __________________________________

Servidor: __________________________________

Porta: ____________________________________

Serviço: __________________________________
```

---

# 📸 Evidências para o GitHub

Recomenda-se registrar capturas das seguintes etapas:

- seleção do MySQL Server;
- seleção do MySQL Workbench;
- verificação dos requisitos;
- configuração do servidor;
- configuração da porta 3306;
- método de autenticação;
- configuração da conta `root`;
- configuração do serviço MySQL80;
- instalação concluída;
- resultado de `mysql --version`;
- acesso ao prompt `mysql>`;
- resultado de `SELECT VERSION()`.

---

# ✅ Checklist

- [ ] MySQL Installer instalado
- [ ] MySQL Server 8 instalado
- [ ] MySQL Workbench instalado
- [ ] Requisitos verificados
- [ ] Servidor Standalone configurado
- [ ] Development Computer selecionado
- [ ] Porta 3306 configurada
- [ ] Autenticação forte configurada
- [ ] Senha do `root` definida
- [ ] Serviço MySQL80 configurado
- [ ] Servidor iniciado
- [ ] `mysql --version` executado
- [ ] Login realizado
- [ ] `SELECT VERSION()` executado
- [ ] `SHOW DATABASES` executado

---

# 🗂️ Estrutura do curso

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
├── 07-JOINs/
│   └── README.md
│
├── 08-Modelagem/
│   └── README.md
│
├── 09-Normalizacao/
│   └── README.md
│
├── 10-Views/
│   └── README.md
│
├── 11-Procedures/
│   └── README.md
│
├── 12-Functions/
│   └── README.md
│
├── 13-Triggers/
│   └── README.md
│
├── 14-Indices-e-Performance/
│   └── README.md
│
├── 15-Usuarios-e-Privilégios/
│   └── README.md
│
├── 16-Seguranca/
│   └── README.md
│
├── 17-Backup-e-Restore/
│   └── README.md
│
└── 18-Projeto-Final/
    └── README.md
```

---

# 📚 Próximas etapas

### Etapa 02
Primeiros comandos e administração básica.

### Etapa 03
Criação de bancos, tabelas e estruturas.

### Etapa 04
Tipos de dados.

### Etapa 05
INSERT, UPDATE e DELETE.

### Etapa 06
SELECT e filtros.

### Etapa 07
JOINs e relacionamentos.

### Etapa 08
Modelagem de banco de dados.

### Etapa 09
Normalização.

### Etapa 10
Views.

### Etapa 11
Stored Procedures.

### Etapa 12
Functions.

### Etapa 13
Triggers.

### Etapa 14
Índices e otimização.

### Etapa 15
Usuários e privilégios.

### Etapa 16
Segurança.

### Etapa 17
Backup e restauração.

### Etapa 18
Projeto final.

---

## 📌 Fonte da primeira etapa

Material fornecido: **Instalação do MySQL 8.0**.

A documentação fornecida apresenta as telas e o fluxo de instalação do MySQL Server 8.0 e MySQL Workbench, incluindo requisitos, configuração do servidor, rede, autenticação, contas, serviço Windows e finalização. fileciteturn16file0L2-L15
