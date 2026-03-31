# Correções do guia prático de recriação

Este arquivo registra os ajustes necessários para que o guia fique executável na prática, sem causar os erros que apareceram durante a reconstrução.

## Ajuste 1: a etapa do Prisma Client precisa vir antes do seed

A sequência correta é:

1. `npm run prisma:generate`
2. `npm run prisma:push`
3. `npm run seed`

Se o seed rodar antes do `prisma generate`, o erro `@prisma/client did not initialize yet` aparece.

## Ajuste 2: o arquivo `prisma/schema.prisma` deve conter o schema real

O arquivo não pode ficar com texto instrucional do guia. Ele precisa receber apenas o schema Prisma válido.

Conteúdo correto:

```prisma
generator client {
	provider = "prisma-client-js"
}

datasource db {
	provider = "mysql"
	url      = env("DATABASE_URL")
}

model Poll {
	id        Int          @id @default(autoincrement())
	title     String       @db.VarChar(255)
	startAt   DateTime
	endAt     DateTime
	createdAt DateTime     @default(now())
	updatedAt DateTime     @updatedAt
	options   PollOption[]

	@@map("polls")
}

model PollOption {
	id        Int      @id @default(autoincrement())
	pollId    Int
	text      String   @db.VarChar(255)
	votes     Int      @default(0)
	createdAt DateTime @default(now())
	updatedAt DateTime @updatedAt
	poll      Poll     @relation(fields: [pollId], references: [id], onDelete: Cascade)

	@@map("poll_options")
}
```

## Ajuste 3: alinhar o schema Prisma com o SQL manual

No SQL manual, `title` e `text` usam `VARCHAR(255)`. No Prisma, para evitar aviso de alteração de tamanho de coluna, os campos devem usar `@db.VarChar(255)`.

Isso evita o aviso de `db push` sobre possível alteração de `VARCHAR(255)` para `VARCHAR(191)`.

## Ajuste 4: corrigir a ordem das etapas 23, 24 e 25

A ordem correta no guia deve ser:

1. Etapa 23: gerar o Prisma Client
2. Etapa 24: aplicar o schema no MySQL
3. Etapa 25: executar o seed

Essa ordem evita o erro do client não inicializado e garante que as tabelas existam antes da inserção dos dados.

## Ajuste 5: corrigir o nome do script

O script correto no `package.json` é:

```json
"prisma:generate": "prisma generate"
```

Não use `npm run prisma:generat`, porque esse nome está incompleto e não existe.

## Ajuste 6: deixar explícito que a etapa 7 não cria conteúdo ainda

Na etapa dos arquivos base, vale reforçar que alguns arquivos podem ser criados vazios primeiro e preenchidos nas etapas seguintes.

Isso evita confusão, principalmente em:

- `prisma/schema.prisma`
- `prisma/seed.js`
- `src/app.js`
- `src/server.js`

## Fluxo validado neste workspace

O fluxo que funcionou aqui foi:

1. `npm run prisma:generate`
2. `npm run prisma:push`
3. `npm run seed`
4. `npm run dev`

Esse é o encadeamento que deve ser usado como referência prática no guia.