# Sessão 13 — React com Vite

**Sessão 13 de 28 · cerca de 6 semanas**

A Estante ganha uma interface em componentes. O Vite sobe o ambiente de desenvolvimento e gera o build. A API continua sendo a da sessão 12: React não substitui o servidor, substitui o DOM feito na mão.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [12 · Frontend e backend juntos](../12-fullstack-html-css-js-node/README.md)
> **Próxima →** [14 · Tailwind](../14-tailwind/README.md)

## O que você vai saber fazer

- Criar um projeto Vite com React.
- Quebrar a tela em componentes que recebem props e guardam estado.
- Buscar dados no efeito certo, com espera e erro.
- Gerar a pasta de produção e saber o que ela contém.

## Documentação

- [Tutorial do React](https://react.dev/learn)
- [Instalação](https://react.dev/learn/installation)
- [Descrevendo a interface](https://react.dev/learn/describing-the-ui)
- [Adicionando interatividade](https://react.dev/learn/adding-interactivity)
- [Gerenciando estado](https://react.dev/learn/managing-state)
- [Efeitos](https://react.dev/learn/synchronizing-with-effects)
- [Você pode não precisar de efeito](https://react.dev/learn/you-might-not-need-an-effect)
- [Guia do Vite](https://vite.dev/guide/)
- [Vite com React](https://vite.dev/guide/#trying-vite-online) — na página do guia, a seção de scaffolding com `npm create vite@latest`

Leia o tutorial do React na ordem em que ele se apresenta. As tarefas aplicam cada bloco na Estante. Se o tutorial usar outro exemplo (tic-tac-toe), faça o exemplo e só então a tarefa correspondente. Os dois cabem nas seis semanas.

## Tarefas

Repositório `estante-react`. A API da sessão 12 precisa estar rodando. Não reescreva a API.

### Tarefa 1 — O que é componente

**Leia:** [descrevendo a interface](https://react.dev/learn/describing-the-ui).

**Faça:** no README, desenhe a árvore: `App`, cabeçalho, lista, ficha, formulário, detalhe. Escreva o que cada um recebe e o que não deve saber (a ficha não busca no servidor; a lista, talvez, também não).

**Pronto quando:** a árvore existe antes do `npm create`.

### Tarefa 2 — Criar o projeto

**Leia:** [guia do Vite](https://vite.dev/guide/).

**Faça:**

```bash
npm create vite@latest estante-react -- --template react
cd estante-react
nvm use
npm install
npm run dev
```

Se o comando de template diferir do guia no dia, siga o guia.

Apague o conteúdo de demonstração até sobrar um `h1` com o nome da Estante.

**Pronto quando:** `npm run dev` abre a sua tela, e o README diz a URL.

### Tarefa 3 — A ficha estática

**Leia:** como passar props, no mesmo capítulo de interface.

**Faça:** `ItemCard` recebe um objeto e renderiza título, ano e estado. `App` tem um array fixo de três itens e usa `map`. A `key` é o `id`, não o índice.

**Pronto quando:** reordenar o array não embaralha estado interno (ainda não há, mas a key já é o id) e o React não avisa de key ausente no console.

### Tarefa 4 — Estado do filtro

**Leia:** [estado](https://react.dev/learn/state-a-components-memory).

**Faça:** botões de filtro guardam o estado no componente pai. A lista derivada é calculada no render (`filter`), não copiada para outro estado.

**Pronto quando:** você leu [you might not need an effect](https://react.dev/learn/you-might-not-need-an-effect) e a filtragem não está dentro de `useEffect`.

### Tarefa 5 — Formulário controlado

**Leia:** [responder a eventos](https://react.dev/learn/responding-to-events) e a parte de formulários do capítulo de estado.

**Faça:** campos de título e ano são controlados. Enviar chama uma função `onAdicionar` recebida por prop. O pai acrescenta no array local. Validação de título vazio aparece na tela.

**Pronto quando:** o campo esvazia só depois do sucesso, e o componente do formulário não conhece a lista inteira.

### Tarefa 6 — Subir e descer estado

**Leia:** [compartilhar estado](https://react.dev/learn/sharing-state-between-components).

**Faça:** avançar e remover sobem até o pai, que altera o array e repassa. Nenhum irmão mexe no outro direto.

**Pronto quando:** você descreve no README onde está "a fonte da verdade" nesta etapa (ainda o array do `App`).

### Tarefa 7 — Efeito para carregar

**Leia:** [sincronizar com efeitos](https://react.dev/learn/synchronizing-with-effects).

**Faça:** substitua o array inicial por `GET /itens`. Estados explícitos: `carregando`, `erro`, `itens`. O efeito tem limpeza se você iniciar um segundo carregamento, ou uma flag de cancelamento, para não gravar resposta velha por cima da nova. Para uma lista que carrega uma vez, o mínimo é não atualizar depois do unmount.

**Pronto quando:** com a API desligada, a UI mostra erro. Com a API ligada, os itens do Postgres aparecem.

### Tarefa 8 — Criar de verdade

**Faça:** `onAdicionar` vira `async`, chama `POST` e inclui na lista o objeto que a API devolveu (com o id do banco, não um id local). Botão desabilitado durante o envio.

**Pronto quando:** o `psql` mostra a linha nova.

### Tarefa 9 — Editar, avançar, apagar, notas

**Faça:** porte os fluxos da sessão 12. A página de detalhe pode ser um estado `itemAberto` no `App`, ou outra rota. Rota ainda não é obrigatória; se quiser, leia a documentação de uma biblioteca pequena ou fique no estado. Escolha o simples e anote.

**Pronto quando:** os quatro fluxos batem com a API.

### Tarefa 10 — Módulo de API

**Faça:** `src/api.js` concentra `listar`, `criar`, `atualizar`, `remover`, `notas`. Os componentes não espalham URL. A base da URL vem de `import.meta.env.VITE_API_URL`, documentada no Vite.

**Pronto quando:** mudar a porta da API é editar o `.env` do Vite (e reiniciar o `dev`, como a documentação do Vite avisa) e um `.env.example` está no Git.

### Tarefa 11 — CSS no componente

**Faça:** traga o visual da sessão 03/10. Pode ser um CSS global no começo e, em seguida, um CSS module na ficha. O foco visível do teclado continua. Não copie um kit de componentes pronto: o objetivo é o React, não uma biblioteca visual.

**Pronto quando:** a página é reconhecível como a sua Estante, em 360px e em tela larga.

### Tarefa 12 — Lista vazia, erro e sucesso

**Faça:** cada região tem os três estados quando faz sentido. Apagar o último item mostra a frase de vazio. Falhar um `POST` não apaga a lista que já estava na tela.

**Pronto quando:** você demonstra os dois casos e descreve no README como reproduzir.

### Tarefa 13 — Chaves, listas e um bug

**Faça:** troque de propósito a `key` para o índice, anote um comportamento estranho se conseguir provocá-lo (um campo de edição dentro da ficha ajuda), e volte a key para o id. Registre no README.

**Pronto quando:** a key definitiva é o id e a história do bug está escrita.

### Tarefa 14 — Build

**Leia:** a seção de build no [guia do Vite](https://vite.dev/guide/).

**Faça:** `npm run build` e `npm run preview`. Abra a pasta `dist/` e diga, no README, o que são o `index.html` e os arquivos com hash. Não commite `dist/` se o README ensina a gerar. Coloque `dist/` no `.gitignore`.

**Pronto quando:** o preview mostra a Estante falando com a API.

### Tarefa 15 — README de dois processos

**Faça:** a ordem: subir Postgres, subir a API da sessão 12, copiar `.env`, `npm install`, `npm run dev`. Diga qual repositório é a API.

**Pronto quando:** essa ordem funciona numa leitura fria, no dia seguinte.

### Tarefa 16 — Opcional: extrair um hook

**Leia:** [reutilizar lógica com hooks](https://react.dev/learn/reusing-logic-with-custom-hooks), se já tiver passado pelo tutorial até lá.

**Faça:** `useItens` devolve `{ itens, carregando, erro, recarregar }` se isso simplificar o `App`. Se o `App` ainda estiver claro sem hook, escreva isso e pule a extração.

**Pronto quando:** ou o hook existe e é usado, ou o README diz por que o `App` ficou como está.

## Checklist

- [ ] 1. Árvore de componentes
- [ ] 2. Vite no ar, demo removida
- [ ] 3. `ItemCard` e `key`
- [ ] 4. Filtro sem efeito
- [ ] 5. Formulário controlado
- [ ] 6. Estado no pai
- [ ] 7. Carga com espera e erro
- [ ] 8. POST devolve o id do banco
- [ ] 9. Editar, avançar, apagar, notas
- [ ] 10. `src/api.js` e `VITE_API_URL`
- [ ] 11. Visual próprio, dois tamanhos
- [ ] 12. Falha no POST preserva a lista
- [ ] 13. Key errada experimentada
- [ ] 14. Build e preview
- [ ] 15. README dos dois processos
- [ ] 16. Opcional: hook

## Próximo passo

Siga para [14 · Tailwind](../14-tailwind/README.md). O visual escrito à mão nesta sessão é o que você vai comparar com as classes utilitárias. O TypeScript entra depois, no Node.
