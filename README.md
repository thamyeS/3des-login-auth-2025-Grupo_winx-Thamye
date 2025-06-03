# Login - auth
Estrutura de autenticação para o projeto **Login**.
## Atividade em grupo de até 3 integrantes.
- Clonar este repositório.
- Testar a api com **insomnia**.
- Estudar e documentar a estrutura do projeto.
- Detalhar e documentar as bibliotecas utilizadas.
- Documentar descrição do funcionamento utilizando **UML DA(Diagrama de Atividades)**.

- ## 🏗️ Estrutura do Projeto

O projeto é estruturado da seguinte forma:

- **/src** – Pasta principal com todo o código da API
  - **/controllers** – Arquivos responsáveis pela lógica das rotas
  - **/routes** – Arquivos que definem as rotas da API
  - **/middlewares** – Arquivos que fazem verificações (ex.: autenticação JWT)
  - **/models** – Arquivos que definem os dados ou interagem com o banco (caso use)
  - **/config** – Arquivos de configuração (ex.: conexão com banco, variáveis)
  - **server.js** – Arquivo principal que sobe o servidor
  
## 📦 Bibliotecas Utilizadas

- **express** – Framework para criar a API de forma simples e rápida
- **jsonwebtoken** – Usado para gerar e validar tokens JWT (autenticação)
- **bcryptjs** – Faz a criptografia de senhas
- **cors** – Permite requisições de outros domínios (libera o acesso externo à API)
- **dotenv** – Carrega variáveis de ambiente do arquivo `.env`
- **nodemon** – Reinicia automaticamente o servidor quando há mudanças no código (desenvolvimento)

## 🔗 Rotas da API

### ✅ POST `/login`
- Descrição: Realiza a autenticação do usuário.
- Retorna um token JWT.

### ✅ POST `/register`
- Descrição: Cadastra um novo usuário com senha criptografada.

### ✅ GET `/profile`
- Descrição: Retorna os dados do usuário autenticado.
- Necessita do token JWT no header.

### ✅ GET `/products`
- Descrição: Lista os produtos cadastrados.

### ✅ POST `/products`
- Descrição: Cadastra um novo produto (necessita estar autenticado).
