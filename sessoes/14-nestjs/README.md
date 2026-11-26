# Sessão 14 — NestJS

**Sessão 14 de 21 · cerca de 5 semanas**

O servidor com `node:http` ensinou o protocolo. O NestJS organiza o que aquele arquivo único já não aguenta: rotas, regra, validação e configuração em caixas com nome. A API da Estante nasce de novo aqui, primeiro em memória. O banco entra com o TypeORM na sessão 15.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [13 · Next.js](../13-nextjs/README.md)
> **Próxima →** [15 · TypeORM](../15-typeorm/README.md)

## O que você vai saber fazer

- Gerar um projeto com a CLI do Nest.
- Separar módulo, controller e service.
- Validar o corpo da requisição com pipe.
- Explicar injeção de dependência com um exemplo do seu código.
- Tratar erro HTTP sem `try/catch` copiado em toda rota.

## Documentação

- [Documentação do NestJS](https://docs.nestjs.com/)
- [Primeiros passos](https://docs.nestjs.com/first-steps)
- [Controllers](https://docs.nestjs.com/controllers)
- [Providers](https://docs.nestjs.com/providers)
- [Módulos](https://docs.nestjs.com/modules)
- [Pipes](https://docs.nestjs.com/pipes)
- [Validação](https://docs.nestjs.com/techniques/validation)
- [Configuração](https://docs.nestjs.com/techniques/configuration)
- [Filtros de exceção](https://docs.nestjs.com/exception-filters)
- [Logger](https://docs.nestjs.com/techniques/logger)
- [Testes](https://docs.nestjs.com/fundamentals/testing)

## Tarefas

Repositório `estante-nest`.

### Tarefa 1 — Por que um framework agora

**Faça:** abra o roteador manual da sessão 09 ou 12 e liste, no README do projeto novo, três responsabilidades misturadas no mesmo arquivo. Ao lado de cada uma, o nome da peça do Nest que deve assumi-la (controller, service, pipe). Você confirma essa coluna na tarefa 8.

**Pronto quando:** a tabela existe antes da instalação.

### Tarefa 2 — CLI e primeiro boot

**Leia:** [primeiros passos](https://docs.nestjs.com/first-steps).

**Faça:** instale a CLI como a documentação mandar e gere o projeto em TypeScript. Suba em modo desenvolvimento. `GET /` responde algo que você reconhece.

**Pronto quando:** a porta está no README e o `curl` recebe a resposta.

### Tarefa 3 — Um recurso, um módulo

**Leia:** [módulos](https://docs.nestjs.com/modules) e [controllers](https://docs.nestjs.com/controllers).

**Faça:** gere o recurso de itens com a CLI (`nest g resource itens`), no estilo REST, **sem** transportar entidade de banco ainda se a CLI perguntar. Se a versão da CLI exigir uma escolha de ORM, escolha a que der para adiar o banco, ou aceite o esqueleto e deixe a entidade quieta até a sessão 15. O objetivo desta sessão é a estrutura.

Implemente `GET /itens` e `GET /itens/:id` sobre um array dentro do service.

**Pronto quando:** os dois `curl` funcionam e o controller não declara o array. Quem guarda o array é o service.

### Tarefa 4 — POST com DTO

**Leia:** [validação](https://docs.nestjs.com/techniques/validation).

**Faça:** `CriarItemDto` com `class-validator`: título obrigatório, tamanho máximo, ano opcional e inteiro. Ligue o `ValidationPipe` global com `whitelist: true`. Status 201 no POST.

**Pronto quando:** um campo extra no JSON (`admin: true`) não aparece no objeto que o service recebe. Você confirma com um log temporário e depois remove o log. Título vazio responde 400 com a mensagem do pipe.

### Tarefa 5 — PATCH e DELETE

**Faça:** DTO de atualização só com os campos permitidos (título, ano, estado). Estado é um conjunto fechado. `DELETE` responde 204. Id ausente lança `NotFoundException`.

**Pronto quando:** o contrato da sessão 09 está coberto, ainda em memória, e `docs/rotas.md` foi atualizado.

### Tarefa 6 — Notas como outro módulo

**Leia:** de novo a página de módulos, a parte de exports e imports.

**Faça:** `NotasModule` com service próprio. O controller pode viver em `itens/:id/notas` ou no módulo de notas, desde que a URL fique estável. O service de notas pede o service de itens para saber se o item existe. Exporte o provider necessário.

**Pronto quando:** nota em id inexistente é 404, e você desenha no README quem importa quem.

### Tarefa 7 — Configuração

**Leia:** [configuration](https://docs.nestjs.com/techniques/configuration).

**Faça:** `ConfigModule` lê `.env`. A porta sai de lá. `.env` no `.gitignore`, `.env.example` no Git. Não há senha de banco ainda; pode deixar a chave `DATABASE_URL` reservada e sem uso.

**Pronto quando:** mudar `PORT` muda o que o Nest escuta.

### Tarefa 8 — Injeção, com as suas palavras

**Faça:** no README, explique injeção olhando o construtor do seu controller: quem cria o service, o que aconteceria se o controller desse `new ItensService()` ele mesmo, e onde o array moraria nesse caso (um array por controller, fácil de duplicar).

**Pronto quando:** o texto cita os arquivos reais.

### Tarefa 9 — Filtro de exceção

**Leia:** [exception filters](https://docs.nestjs.com/exception-filters).

**Faça:** um erro inesperado (lance um `Error` genérico num caminho de teste) vira resposta JSON com mensagem genérica. O detalhe fica no log. As `HttpException` continuam com o status certo.

**Pronto quando:** você viu os dois formatos de resposta.

### Tarefa 10 — Logger

**Leia:** [logger](https://docs.nestjs.com/techniques/logger).

**Faça:** troque `console.log` pelo `Logger` do Nest no service, no nível que fizer sentido (`log` ao criar, `warn` ao recusar). Não logue o corpo inteiro se um dia ele tiver senha; hoje, evite logar o item completo em produção e já crie o hábito.

**Pronto quando:** criar um item deixa uma linha identificável no terminal, com o id.

### Tarefa 11 — Teste do service

**Leia:** [testing](https://docs.nestjs.com/fundamentals/testing).

**Faça:** teste unitário do service: criar recusa título vazio (se a regra estiver no service além do pipe), buscar id ausente lança `NotFoundException`, listar devolve o que foi criado. Sem subir a porta.

**Pronto quando:** `npm test` passa.

### Tarefa 12 — Teste HTTP

**Faça:** um teste e2e da CLI do Nest para `POST` inválido (400) e `GET` depois de um `POST` válido. Pode usar o utilitário que o projeto gerado já traz.

**Pronto quando:** o teste sobe o app em memória e não depende do Postgres.

### Tarefa 13 — Pasta que você consegue explicar

**Faça:** no README, a árvore real com uma linha por pasta (`itens`, `notas`, `common` se existir). Se algo ficou em `app.controller.ts` sem necessidade, mova para um módulo.

**Pronto quando:** cada arquivo de `src/` tem um dono claro.

### Tarefa 14 — README de API

**Faça:** como instalar, como rodar, a tabela de rotas, e a frase: os dados ainda somem quando o processo reinicia. A sessão 15 corrige isso.

**Pronto quando:** o `curl` do README cria um item de verdade.

## Checklist

- [ ] 1. Três misturas do servidor antigo
- [ ] 2. Projeto gerado e no ar
- [ ] 3. Módulo de itens
- [ ] 4. DTO com whitelist
- [ ] 5. PATCH, DELETE e 404
- [ ] 6. Módulo de notas
- [ ] 7. Porta via configuração
- [ ] 8. Injeção explicada
- [ ] 9. Erro inesperado sem detalhe interno
- [ ] 10. Logger no lugar do console
- [ ] 11. Teste de service
- [ ] 12. Teste HTTP
- [ ] 13. Árvore explicada
- [ ] 14. README avisa que a memória é temporária

## Próximo passo

Siga para [15 · TypeORM](../15-typeorm/README.md).
