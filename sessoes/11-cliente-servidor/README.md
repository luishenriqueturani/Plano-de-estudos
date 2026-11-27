# Sessão 11 — Cliente e servidor

**Sessão 11 de 27 · cerca de 2 semanas**

Até aqui, quem mexe nos dados é você, no terminal. Um site precisa de outro arranjo: o navegador pede, um processo responde. Esta sessão dá nome a esse arranjo e implementa uma API mínima com o `node:http`, sem framework. A tela bonita que consome a API é a sessão 12.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [10 · Docker e PostgreSQL](../10-docker-e-postgresql/README.md)
> **Próxima →** [12 · Frontend e backend juntos](../12-fullstack-html-css-js-node/README.md)

## O que você vai saber fazer

- Ler uma requisição HTTP no DevTools e no `curl`.
- Dizer o que são método, caminho, cabeçalho, corpo e status.
- Responder JSON num servidor escrito com `node:http`.
- Escrever o contrato das rotas da Estante antes de implementar todas.

## Documentação

- [Visão geral de HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Overview)
- [Métodos](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Methods)
- [Códigos de status](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status)
- [Cabeçalhos](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Headers)
- [HTTP no Node](https://nodejs.org/api/http.html)
- [CORS](https://developer.mozilla.org/pt-BR/docs/Glossary/CORS)
- [curl](https://curl.se/docs/manual.html)

## Tarefas

Repositório novo: `estante-api`. O Compose do Postgres pode ser copiado da sessão 10. Comece a API em memória (um array) e só então ligue o banco, para o HTTP não se misturar com SQL no primeiro dia.

### Tarefa 1 — Quem é o cliente e quem é o servidor

**Faça:** um desenho no README. Caixas: navegador, processo Node, Postgres. Setas com o que viaja em cada uma (HTML, JSON, SQL). Marque a porta do Node e a porta do Postgres.

**Pronto quando:** o desenho mostra que o navegador não fala SQL.

### Tarefa 2 — Ver uma requisição de verdade

**Faça:** abra qualquer site na rede em que você já navega, DevTools, aba Rede, recarregue. Escolha um documento e um pedido de API ou de arquivo. Anote método, status e dois cabeçalhos.

**Pronto quando:** a nota distingue o pedido da página do pedido de um recurso a mais (imagem, script ou JSON).

### Tarefa 3 — Servidor que responde texto

**Leia:** o exemplo de servidor em [http](https://nodejs.org/api/http.html).

**Faça:** `src/servidor.js` escuta a porta `3000`. `GET /` responde `text/plain` com o nome da API. Qualquer outro caminho responde 404 com uma frase.

**Pronto quando:** `curl -i http://localhost:3000/` mostra `200` e o cabeçalho de tipo, e um caminho inventado mostra `404`.

### Tarefa 4 — JSON e status certos

**Faça:** um array em memória com dois itens. `GET /itens` responde `application/json` e status 200. `GET /itens/1` responde o item. Id ausente responde 404 com `{ "erro": "item não encontrado" }`.

**Pronto quando:** o `curl` mostra o cabeçalho `Content-Type` correto nos dois casos.

### Tarefa 5 — Ler o corpo

**Leia:** como o objeto `req` emite dados, na documentação de `http.IncomingMessage`.

**Faça:** `POST /itens` lê o corpo, exige JSON com título e responde 201 com o item criado, inclusive o `id`. JSON inválido responde 400. Título vazio responde 400. Método que você não implementou responde 405.

**Pronto quando:** os três erros têm teste manual com `curl`, anotado no README com o comando e o status.

### Tarefa 6 — O contrato

**Faça:** `docs/rotas.md` com uma tabela, antes de implementar o que ainda falta:

| Método | Caminho | Corpo | Sucesso | Erros |
| --- | --- | --- | --- | --- |
| GET | /itens | — | 200 lista | — |
| GET | /itens/:id | — | 200 item | 404 |
| POST | /itens | titulo, ano | 201 item | 400 |
| PATCH | /itens/:id | estado ou titulo | 200 | 400, 404 |
| DELETE | /itens/:id | — | 204 | 404 |
| POST | /itens/:id/notas | texto | 201 | 400, 404 |
| GET | /itens/:id/notas | — | 200 | 404 |

Ajuste se a sua Estante tiver outro campo, mas não deixe rota sem status de erro.

**Pronto quando:** a tabela existe e você ainda não implementou `PATCH`. A ordem importa: o contrato guia o código.

### Tarefa 7 — Implementar o contrato em memória

**Faça:** complete as rotas da tabela sobre o array. `DELETE` bem-sucedido não precisa de corpo; 204 é adequado. Estado fora da lista permitida é 400.

**Pronto quando:** existe um arquivo `docs/manual.md` com um `curl` por rota, e você rodou todos.

### Tarefa 8 — Ligar o Postgres

**Faça:** troque o array pelas funções da sessão 10. Suba o Compose antes do Node. Se o banco estiver fora, a API responde 500 com uma mensagem genérica e registra o detalhe no terminal do servidor, não no JSON.

**Pronto quando:** reiniciar o Node não perde os itens, porque eles estão no banco. Reiniciar sem o Compose faz o `GET /itens` falhar com a mensagem genérica, e o terminal mostra a causa.

### Tarefa 9 — Uma rota, uma função

**Faça:** o `createServer` só decide a rota e chama uma função. A função devolve status e corpo. Um teste chama essa função sem abrir porta (passe o pedido já interpretado).

**Pronto quando:** pelo menos o caso "título vazio → 400" é um teste automático, sem `curl`.

### Tarefa 10 — CORS, só o conceito e o sintoma

**Leia:** [CORS](https://developer.mozilla.org/pt-BR/docs/Glossary/CORS).

**Faça:** abra uma página local (pode ser a da sessão 04) que tenta `fetch('http://localhost:3000/itens')`. Observe o erro no console. Depois responda o cabeçalho `Access-Control-Allow-Origin` com `http://127.0.0.1:PORTA` ou a origem exata que o navegador mostrou, só para o `GET`. Anote por que você não usou `*` como hábito para o resto da vida: na sessão 22 a API passa a carregar credencial, e origem aberta junto de credencial é uma combinação que a documentação do Nest também alerta para evitar.

**Pronto quando:** o `GET` a partir da página funciona, o README explica o erro que existia antes, e o `POST` cross-origin pode continuar falhando. Completar o preflight é a tarefa 11.

### Tarefa 11 — Preflight

**Leia:** a seção de requisição preflight no glossário de CORS ou no guia de HTTP da MDN.

**Faça:** `OPTIONS` responde os métodos e cabeçalhos que o `POST` precisa (`Content-Type`). O `fetch` da página de teste cria um item.

**Pronto quando:** a aba Rede mostra o `OPTIONS` com sucesso e, em seguida, o `POST` 201.

### Tarefa 12 — O que ficou frágil de propósito

**Faça:** no README, uma lista honesta:

- não há autenticação: quem alcança a porta manda no dado;
- o roteamento é manual;
- não há validação além dos campos que você lembrou.

Esses três pontos são o motivo das sessões 14 e 18. Não tente resolvê-los agora com um framework.

**Pronto quando:** a lista está no README e a API continua simples.

## Checklist

- [ ] 1. Desenho com portas
- [ ] 2. Uma requisição real anotada
- [ ] 3. `GET /` e 404
- [ ] 4. JSON com 200 e 404
- [ ] 5. POST com 201 e 400
- [ ] 6. Contrato escrito antes do PATCH
- [ ] 7. `curl` de todas as rotas
- [ ] 8. Postgres, com 500 genérico se cair
- [ ] 9. Teste sem abrir porta
- [ ] 10. CORS entendido num GET
- [ ] 11. Preflight do POST
- [ ] 12. Limitações assumidas no README

## Próximo passo

Siga para [12 · Frontend e backend juntos](../12-fullstack-html-css-js-node/README.md).
