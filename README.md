# Sistema de Votação

Sistema de votação desenvolvido em Node.js com Fastify, MySQL, Prisma e Socket.IO. O projeto atende ao CRUD completo de enquetes, votação em tempo real e interface web servida pelo próprio backend.

## Visão geral

Este projeto foi criado para uma avaliação técnica com foco em:

- backend em Node.js
- banco MySQL
- CRUD de enquetes
- opções dinâmicas com mínimo de 3 respostas
- listagem por status: não iniciada, em andamento e finalizada
- votação em tempo real
- front-end responsivo

## Funcionalidades

- criar enquete com título, data de início e data de término
- editar enquete existente
- excluir enquete
- listar todas as enquetes cadastradas
- visualizar detalhes de uma enquete
- votar em uma opção ativa
- atualizar resultados em tempo real com Socket.IO
- validar a API com arquivo HTTP de testes
- criar seed inicial para facilitar a demonstração

## Stack utilizada

- Node.js
- Fastify
- Prisma
- MySQL
- Socket.IO
- HTML, CSS e JavaScript puro

## Estrutura do projeto

```text
package.json
.env
.gitignore
api-tests.http
database/
  schema.sql
prisma/
  schema.prisma
  seed.js
public/
  app.js
  index.html
  styles.css
src/
  app.js
  server.js
  config/
    prisma.js
    socket.js
  controllers/
    pollController.js
  middlewares/
    errorHandler.js
  repositories/
    pollRepository.js
  routes/
    pollRoutes.js
  services/
    pollService.js
```

## Pré-requisitos

Antes de rodar o projeto, você precisa ter:

- Node.js instalado
- MySQL instalado localmente ou via XAMPP
- VS Code instalado
- terminal disponível no VS Code

## Configuração do banco

O projeto usa MySQL com a seguinte configuração padrão:

- host: `localhost`
- porta: `3306`
- banco: `sistema_votacao`
- usuário: `root`
- senha: vazia no XAMPP local

## Variáveis de ambiente

Crie o arquivo `.env` na raiz do projeto com:

```env
PORT=3000
DATABASE_URL="mysql://root@localhost:3306/sistema_votacao"
```

## Instalação

```bash
npm install
```

## Comandos principais

Gere o Prisma Client:

```bash
npm run prisma:generate
```

Aplique o schema no banco:

```bash
npm run prisma:push
```

Execute o seed inicial:

```bash
npm run seed
```

Inicie o servidor em modo desenvolvimento:

```bash
npm run dev
```

Inicie sem recarga automática:

```bash
npm start
```

Abra o Prisma Studio:

```bash
npm run prisma:studio
```

## Ordem recomendada de execução

1. iniciar o MySQL
2. gerar o Prisma Client
3. aplicar o schema no banco
4. executar o seed
5. subir o servidor

## Fluxo de desenvolvimento

O projeto foi organizado em camadas:

- `repositories`: acesso ao banco
- `services`: regras de negócio e validação
- `controllers`: ponte entre HTTP e service
- `routes`: definição dos endpoints
- `middlewares`: tratamento de erro
- `config`: Prisma e Socket.IO

## API

### Health check

```http
GET /api/health
```

### Enquetes

```http
GET /api/polls
GET /api/polls/:id
POST /api/polls
PUT /api/polls/:id
DELETE /api/polls/:id
POST /api/polls/:id/vote
```

## Regras de negócio

- a enquete precisa ter título
- a enquete precisa ter no mínimo 3 opções
- a data de início precisa ser anterior à data de término
- a votação só é permitida quando a enquete está em andamento
- os totais são atualizados em tempo real

## Front-end

A interface web é servida pelo próprio backend e permite:

- cadastrar enquete
- editar enquete
- excluir enquete
- listar enquetes
- votar em opções ativas
- acompanhar atualizações em tempo real

## Testes da API

O arquivo [api-tests.http](api-tests.http) reúne chamadas para validar:

- health check
- listagem
- busca por ID
- criação
- atualização
- voto
- exclusão

## Seed

O seed cria uma enquete inicial para facilitar testes rápidos depois do `db push`.

## Banco manual

O arquivo [database/schema.sql](database/schema.sql) documenta a estrutura das tabelas em SQL puro.

## Problemas comuns

### O erro diz que `@prisma/client` não inicializou

Rode:

```bash
npm run prisma:generate
```

### O erro diz que não consegue conectar ao banco

Verifique:

- MySQL ligado no XAMPP
- porta correta no `.env`
- banco existente
- credenciais corretas

### A interface abre em branco

Verifique se:

- o servidor está rodando
- `public/index.html` existe
- `src/app.js` está registrando os arquivos estáticos

## Demonstração do projeto

Quando a aplicação estiver rodando, acesse:

```text
http://localhost:3000
```

## Resultado esperado

Ao final, você terá:

- backend funcionando em Node.js
- MySQL integrado via Prisma
- CRUD completo de enquetes
- votação em tempo real
- interface responsiva
- seed inicial para teste
