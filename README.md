# Automação de Testes de API REST

Projeto de automação de testes de API REST desenvolvido utilizando **Postman e Newman**, com integração contínua por meio do **GitHub Actions**.

O objetivo é validar operações CRUD, regras de cadastro, cenários negativos e autenticação JWT, garantindo a execução automatizada dos testes e a geração de evidências em relatórios HTML.

## Tecnologias utilizadas

- **Postman:** criação e organização das requisições e scripts de teste.
- **Newman:** execução automatizada das collections via linha de comando.
- **JavaScript:** implementação das validações e manipulação de variáveis.
- **Node.js e npm:** gerenciamento das dependências.
- **Git e GitHub:** versionamento do projeto.
- **GitHub Actions:** integração contínua.
- **Newman Reporter HTML Extra:** geração de relatórios HTML.

## API utilizada

Os testes utilizam a API pública [ServeRest](https://serverest.dev), disponibilizada para estudos e práticas de testes automatizados.

**Base URL:** `https://serverest.dev`

## Cenários automatizados

### 1. CRUD de usuários

Validação das principais operações de gerenciamento de usuários:

- Criar usuário.
- Listar usuários.
- Consultar usuário pelo ID.
- Atualizar informações cadastrais.
- Validar os dados atualizados.
- Excluir usuário.
- Confirmar a exclusão.

Os testes utilizam variáveis para compartilhar identificadores entre as requisições e validar o comportamento da API ao longo do fluxo.

### 2. Cenários negativos

Validação de regras de cadastro, incluindo:

- Nome obrigatório.
- E-mail obrigatório.
- Senha obrigatória.
- Campo administrador obrigatório.
- Tentativa de cadastro com e-mail duplicado.

Os testes de campos obrigatórios utilizam a abordagem **Data-Driven**, com dados armazenados em arquivo JSON.

### 3. Autenticação JWT

Validação dos controles de autenticação e autorização:

- Preparação de usuário para autenticação.
- Login com credenciais válidas.
- Recebimento e armazenamento do token Bearer.
- Tentativa de acesso a recurso protegido sem token.
- Acesso autorizado utilizando token válido.
- Exclusão dos dados criados durante os testes.

## Estrutura do projeto

```text
api-automation-tests/
├── .github/
│   └── workflows/
│       └── api-tests.yml
├── .gitignore
├── README.md
├── dados-cadastro-negativo.json
├── package.json
├── package-lock.json
└── postman-api-automation-tests.postman_collection.json
```

## Pré-requisitos

- Node.js 22 ou versão compatível.
- npm.
- Acesso à internet para comunicação com a API ServeRest.

## Instalação

Clone o repositório:

```bash
git clone https://github.com/ThaisGodoy/api-automation-tests.git
```

Acesse a pasta:

```bash
cd api-automation-tests
```

Instale as dependências:

```bash
npm ci
```

## Execução dos testes

### CRUD

```bash
npx newman run postman-api-automation-tests.postman_collection.json --folder "CRUD"
```

### Autenticação JWT

```bash
npx newman run postman-api-automation-tests.postman_collection.json --folder "JWT"
```

### Cenários negativos — Data-Driven

```bash
npx newman run postman-api-automation-tests.postman_collection.json \
  --folder "Validações de cadastro" \
  --iteration-data dados-cadastro-negativo.json
```

### E-mail duplicado

```bash
npx newman run postman-api-automation-tests.postman_collection.json \
  --folder "Email duplicado"
```

## Relatórios HTML

Para executar o CRUD e gerar um relatório HTML:

```bash
mkdir -p reports

npx newman run postman-api-automation-tests.postman_collection.json \
  --folder "CRUD" \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/crud.html
```

Os relatórios permitem consultar os resultados das assertions, os tempos de resposta e eventuais falhas.

A pasta `reports/` é ignorada pelo Git, pois contém arquivos gerados durante a execução.

## Integração contínua — GitHub Actions

O projeto possui uma pipeline configurada em:

`.github/workflows/api-tests.yml`

A pipeline é acionada automaticamente em eventos de `push` e `pull_request`, além de permitir execução manual.

O workflow realiza:

1. Checkout do repositório.
2. Configuração do Node.js.
3. Instalação das dependências.
4. Execução dos testes CRUD.
5. Execução dos testes JWT.
6. Execução dos cenários negativos e Data-Driven.
7. Validação de e-mail duplicado.
8. Geração e publicação dos relatórios HTML como artefatos.

Os relatórios podem ser baixados na aba **Actions** do GitHub, dentro da execução correspondente.

## Estratégia de testes

O projeto utiliza:

- Testes funcionais de API REST.
- Testes positivos e negativos.
- Validação de códigos de status HTTP.
- Validação de mensagens e estruturas JSON.
- Validação de autenticação e autorização.
- Testes orientados a dados (Data-Driven).
- Encadeamento de requisições utilizando variáveis.
- Preparação e limpeza de dados de teste.
- Execução automatizada em CI.

