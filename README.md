
# Login - auth
Estrutura de autenticação para o projeto **Login**.
## Atividade em grupo. (Winx- Thamye, Rebeca e Steffany)
- Clonar este repositório.
- Testar a api com **insomnia**.
- Estudar e documentar a estrutura do projeto.
- Detalhar e documentar as bibliotecas utilizadas.
- Documentar descrição do funcionamento utilizando **UML DA(Diagrama de Atividades)**.

## Bibliotecas Utilizadas

- **/src** – Pasta principal com todo o código da API
  - **/controllers** – Arquivos responsáveis pela lógica das rotas
  - **/routes** – Arquivos que definem as rotas da API
  - **/middlewares** – Arquivos que fazem verificações (ex.: autenticação JWT)
  - **server.js** – Arquivo principal que sobe o servidor
  
##  Bibliotecas Utilizadas

- **express** – Framework para criar a API de forma simples e rápida
- **jsonwebtoken** – Usado para gerar e validar tokens JWT (autenticação)
- **cors** – Permite requisições de outros domínios (libera o acesso externo à API)
- **dotenv** – Carrega variáveis de ambiente do arquivo `.env`
- **nodemon** – Reinicia automaticamente o servidor quando há mudanças no código (desenvolvimento)

##  Rotas da API

###  POST `/login`
- Descrição: Realiza a autenticação do usuário.
- Retorna um token JWT.

###  GET `/posts`
- Descrição: Verifica o token JWT.
- Retorna todos os dados cadastrados.


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

### Descrição do Diagrama

O diagrama ilustra:
1.  O processo de login, onde o usuário envia credenciais e recebe um JWT se forem válidas.
2.  O processo de acesso aos posts, onde o middleware valida o JWT antes de permitir que o controlador retorne os dados.

## Considerações

*   **Segurança**: A validação de credenciais hardcoded é insegura. Recomenda-se usar hashes de senha armazenados em banco de dados.
*   **Tratamento de Erros**: O tratamento de erros na validação do token pode ser aprimorado para usar códigos de status HTTP mais específicos (401/403).
*   **Dados**: Os posts são carregados de um arquivo local; em produção, um banco de dados seria mais apropriado.
