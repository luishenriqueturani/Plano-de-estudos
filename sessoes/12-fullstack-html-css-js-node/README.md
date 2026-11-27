# Sessão 12 — Frontend e backend juntos

**Sessão 12 de 27 · cerca de 5 semanas**

A página da sessão 04 e a API da sessão 11 passam a ser um produto só: HTML, CSS e JavaScript no navegador, Node no servidor, PostgreSQL no Docker. Incluir um item na tela grava no banco. Recarregar em outro navegador mostra o mesmo item, porque o `localStorage` deixou de ser a fonte.

Repositório sugerido: `estante-web`, copiando a API para `servidor/` e o visual para `publico/`. Um README só, na raiz, explica os dois processos.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [11 · Cliente e servidor](../11-cliente-servidor/README.md)
> **Próxima →** [13 · React com Vite](../13-react-com-vite/README.md)

## O que você vai saber fazer

- Servir a página e a API no mesmo processo, ou em dois, com a escolha escrita.
- Listar, criar, editar e apagar passando pelo servidor.
- Mostrar espera, vazio e erro sem quebrar a página.
- Subir o ambiente a partir do README: Compose, `.env`, Node.

## Documentação

- [fetch](https://developer.mozilla.org/pt-BR/docs/Web/API/Fetch_API)
- [Usar Fetch](https://developer.mozilla.org/pt-BR/docs/Web/API/Fetch_API/Using_Fetch)
- [Formulários](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Forms)
- [node:http](https://nodejs.org/api/http.html), para servir arquivo estático se você ficar num processo só
- O contrato `docs/rotas.md` que você escreveu na sessão 11

## Tarefas

### Tarefa 1 — Desenhar o clique

**Faça:** no README, o caminho de "cliquei em adicionar": navegador, rota, função, SQL, resposta, o que a tela faz com o JSON. Uma linha por salto.

**Pronto quando:** o caminho inclui o status de erro, não só o caso feliz.

### Tarefa 2 — Servir o frontend

**Faça:** escolha um e registre:

- o mesmo Node serve `publico/` e as rotas `/itens`, ou
- um servidor estático simples na porta 5173 (pode ser `npx serve`, se você ler o que o comando faz) e a API na 3000, com o CORS da sessão 11.

**Pronto quando:** abrir a URL do frontend mostra o HTML da Estante, ainda que a lista esteja vazia.

### Tarefa 3 — Listar

**Faça:** ao carregar, `fetch` em `GET /itens`. Enquanto a resposta não chega, a página mostra "carregando". Lista vazia mostra uma frase, não uma grade em branco. Erro de rede mostra uma frase e um botão de tentar de novo.

**Pronto quando:** com o servidor desligado, a página mostra o erro em vez de ficar em "carregando" para sempre.

### Tarefa 4 — Criar

**Faça:** o formulário envia JSON. Sucesso: a lista passa a incluir o item devolvido (busque de novo ou insira o JSON na lista local). Falha 400: a mensagem do servidor aparece ao lado do campo. Os botões ficam desabilitados enquanto o pedido viaja.

**Pronto quando:** dois cliques rápidos não criam dois itens.

### Tarefa 5 — Editar título e ano

**Faça:** `PATCH` com os campos alterados. A tela entra num modo de edição da ficha ou numa página de detalhe. Cancelar não chama a API.

**Pronto quando:** recarregar mostra o valor novo, vindo do banco.

### Tarefa 6 — Avançar estado e apagar

**Faça:** avançar chama o `PATCH`. Apagar pede confirmação (`confirm` é aceitável aqui) e chama `DELETE`. Se o servidor responder 404, a lista é recarregada: alguém já apagou.

**Pronto quando:** os dois fluxos funcionam com o banco aberto no `psql` ao lado, e você vê a linha mudar.

### Tarefa 7 — Notas no detalhe

**Faça:** a página do item busca `GET /itens/:id` e `GET /itens/:id/notas`. O formulário de nota faz `POST`. Id inexistente mostra 404 amigável.

**Pronto quando:** uma nota gravada sobrevive a um restart do Node.

### Tarefa 8 — Busca e filtro

**Faça:** filtro de estado e texto. Decida, e escreva no README, se o filtro roda no navegador sobre a lista já carregada ou se vira query na API (`GET /itens?estado=fazendo`). As duas valem. Query na API é o melhor exercício se a coleção ainda é pequena: você implementa a query do mesmo jeito.

**Pronto quando:** a decisão está escrita e o comportamento combina com ela.

### Tarefa 9 — Validação nos dois lados

**Faça:** o navegador valida para a pessoa não esperar. O servidor valida de novo e é o que vale. Desligue o JavaScript do formulário num teste: chame a API com `curl` e título vazio, e confirme o 400. Escreva no README a frase: validação no navegador não é segurança.

**Pronto quando:** o `curl` do título vazio está no README, com a resposta.

### Tarefa 10 — Configuração

**Faça:** porta, origem do CORS e dados do Postgres vêm de `.env`. Use um arquivo carregado por você (leia a documentação de `process.env` e, se quiser um carregador, a do pacote que escolher, sem commitar segredo). `.env.example` lista as chaves.

**Pronto quando:** mudar a porta no `.env` muda de fato, e `git status` não mostra o `.env`.

### Tarefa 11 — Commits por comportamento

**Faça:** se você acumulou tudo num commit só, pare. Daqui para a frente, cada tarefa desta sessão é um commit cujo assunto é o comportamento ("mostra erro quando a API está fora"). Não reescreva o passado com rebase.

**Pronto quando:** `git log --oneline` desta fase tem mais de um commit legível.

### Tarefa 12 — README único

**Faça:** ordem de execução:

1. copiar `.env.example`;
2. `docker compose up -d`;
3. aplicar o schema, se o volume for novo;
4. `nvm use`;
5. `npm install`;
6. `npm start`;
7. a URL para abrir.

Inclua como rodar os testes do servidor.

**Pronto quando:** clonar em outra pasta e seguir o README chega na lista.

### Tarefa 13 — Passar o mouse na arquitetura

**Faça:** responda no README, em um parágrafo cada:

- o que quebra se o Postgres cair;
- o que quebra se só o Node cair;
- o que ainda funciona se o JavaScript do navegador estiver desligado (a página pode ser só a casca: assuma isso com honestidade);
- onde está a regra "não passa de `feito`": no navegador, no Node, no `CHECK` do banco, ou nos três.

**Pronto quando:** a última resposta cita o arquivo ou o SQL de cada camada que realmente tem a regra.

### Tarefa 14 — Opcional: ordenar

**Faça:** a lista aceita `?ordem=titulo` ou `?ordem=recente`, refletida num controle da página.

**Pronto quando:** a URL ou o controle deixa claro a ordem atual.

## Checklist

- [ ] 1. Caminho do clique escrito
- [ ] 2. Frontend servido
- [ ] 3. Lista com carregando, vazio e erro
- [ ] 4. Criar sem duplicar no clique duplo
- [ ] 5. Editar
- [ ] 6. Avançar e apagar
- [ ] 7. Notas no detalhe
- [ ] 8. Filtro com a decisão documentada
- [ ] 9. Servidor recusa o que o navegador já recusaria
- [ ] 10. `.env` fora do Git
- [ ] 11. Commits por comportamento
- [ ] 12. README seguido numa pasta nova
- [ ] 13. Quatro perguntas de arquitetura
- [ ] 14. Opcional: ordenação

## Próximo passo

Siga para [13 · React com Vite](../13-react-com-vite/README.md). A API desta sessão permanece no ar. O React entra no lugar do DOM manual.
