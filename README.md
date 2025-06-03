# Documentação Detalhada da API

Esta seção fornece uma análise detalhada da API de autenticação e posts.

## Bibliotecas Utilizadas

O projeto utiliza as seguintes bibliotecas Node.js, conforme listado no arquivo `package.json`:

*   **dotenv**: Carrega variáveis de ambiente de um arquivo `.env` para `process.env`, essencial para gerenciar configurações sensíveis como a chave secreta do JWT fora do código-fonte.
*   **Express**: Framework web minimalista para Node.js, utilizado como a espinha dorsal da API para definir rotas (`/login`, `/posts`), gerenciar requisições/respostas e integrar middlewares.
*   **jsonwebtoken**: Implementa o padrão JSON Web Token (JWT) para criar tokens seguros após o login e verificar tokens em requisições a rotas protegidas, garantindo a autenticação do usuário.

## Funcionamento das Rotas

A API expõe duas rotas principais:

### 1. Autenticação (`POST /login`)

*   **Controlador**: `src/controllers/login.js`
*   **Fluxo**: Recebe `user` (email) e `psw` (senha) no corpo da requisição. Valida as credenciais contra valores fixos no código (prática insegura para produção). Se as credenciais forem válidas, gera um JWT assinado com a `SECRET_JWT` (definida via `.env`) e com expiração de 2 minutos, contendo um ID de usuário aleatório, nome e avatar fixos. Retorna o token com status 200. Se inválidas, retorna 401.

### 2. Acesso aos Posts (`GET /posts`)

*   **Controlador**: `src/controllers/posts.js`
*   **Middleware**: `src/middlewares/auth.js`
*   **Fluxo**: Esta rota é protegida pelo middleware `validate`. O middleware extrai o token JWT do cabeçalho `Authorization`. Se o token estiver ausente, retorna 401. Se presente, verifica sua validade (assinatura e expiração) usando a `SECRET_JWT`. Se inválido, retorna 500 (idealmente seria 401/403). Se válido, anexa o payload do token à requisição e passa o controle ao controlador `posts`. O controlador lê dados de posts de `src/data/posts.js` e os retorna com status 200.

## Diagrama de Atividades UML

O fluxo principal da API, incluindo login e acesso aos posts, pode ser visualizado no diagrama de atividades abaixo (descrito em sintaxe PlantUML):

```plantuml
@startuml Diagrama de Atividades - API Winx

title Fluxo de Atividades: Autenticação e Acesso a Posts

|Usuário|          |API (Express)|            |Middleware Auth|

start

' Fluxo de Login
|Usuário|
:Inicia requisição POST /login;
:Envia credenciais (email, senha);

|API (Express)|
:Recebe requisição /login;
:Controlador Login processa;
:Verifica credenciais (hardcoded);

if (Credenciais válidas?) then (Sim)
  :Gera JWT (payload, segredo, expiração);
  :Retorna resposta 200 OK com JWT;
  |Usuário|
  :Recebe e armazena JWT;
else (Não)
  |API (Express)|
  :Retorna resposta 401 Unauthorized;
  |Usuário|
  :Recebe erro de login;
  stop
endif

' Fluxo de Acesso a Posts
|Usuário|
:Inicia requisição GET /posts;
:Envia JWT no cabeçalho Authorization;

|API (Express)|
:Recebe requisição /posts;

|Middleware Auth|
:Intercepta requisição;
:Extrai token do cabeçalho;
if (Token presente?) then (Sim)
  :Verifica assinatura e validade do token (usando segredo);
  if (Token válido?) then (Sim)
    :Anexa payload do usuário à requisição;
    :Chama next();
    |API (Express)|
    :Controlador Posts processa;
    :Lê dados dos posts (data/posts.js);
    :Retorna resposta 200 OK com posts;
    |Usuário|
    :Recebe dados dos posts;
    stop
  else (Não)
    |Middleware Auth|
    :Retorna resposta 500 Internal Server Error (Erro na validação);
    ' Nota: Deveria ser 401 ou 403
    |Usuário|
    :Recebe erro de acesso;
    stop
  endif
else (Não)
  |Middleware Auth|
  :Retorna resposta 401 Unauthorized (Token ausente);
  |Usuário|
  :Recebe erro de acesso;
  stop
endif

@enduml
```

### Descrição do Diagrama

O diagrama ilustra:
1.  O processo de login, onde o usuário envia credenciais e recebe um JWT se forem válidas.
2.  O processo de acesso aos posts, onde o middleware valida o JWT antes de permitir que o controlador retorne os dados.

## Considerações

*   **Segurança**: A validação de credenciais hardcoded é insegura. Recomenda-se usar hashes de senha armazenados em banco de dados.
*   **Tratamento de Erros**: O tratamento de erros na validação do token pode ser aprimorado para usar códigos de status HTTP mais específicos (401/403).
*   **Dados**: Os posts são carregados de um arquivo local; em produção, um banco de dados seria mais apropriado.

