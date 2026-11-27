# Sessão 14 — Tailwind

**Sessão 14 de 27 · cerca de 2 semanas**

O CSS da sessão 13 foi escrito à mão, de propósito. Tailwind é o outro extremo: a maior parte do visual mora em classes no próprio componente, com um vocabulário fixo de espaço, cor, tipo e tela. Você não está trocando de produto. Está vestindo a mesma Estante de outro jeito, para sentir o que a ferramenta economiza e o que ela esconde.

O Next.js, na sessão 16, usa este mesmo vocabulário. Por isso ele entra agora, antes do TypeScript: tipo e classe utilitária são assuntos diferentes, e misturar os dois no mesmo semana atrapalha.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [13 · React com Vite](../13-react-com-vite/README.md)
> **Próxima →** [15 · TypeScript com Node](../15-typescript-com-node/README.md)

## O que você vai saber fazer

- Instalar o Tailwind no projeto Vite que já existe, pelo guia oficial.
- Traduzir espaçamento, cor, flex e grid para classes.
- Fazer a tela estreita primeiro, com os prefixos de largura.
- Manter foco visível no teclado.
- Decidir o que continua em CSS seu, se sobrar alguma coisa.

## Documentação

- [Instalação com Vite](https://tailwindcss.com/docs/installation/using-vite)
- [Documentação do Tailwind](https://tailwindcss.com/docs)
- [Estilizando com classes](https://tailwindcss.com/docs/styling-with-utility-classes) — se o título mudar, comece pelo índice em [tailwindcss.com/docs](https://tailwindcss.com/docs)
- [Design responsivo](https://tailwindcss.com/docs/responsive-design)
- [Hover, focus e outros estados](https://tailwindcss.com/docs/hover-focus-and-other-states)
- [Reusando estilos](https://tailwindcss.com/docs/reusing-styles)

Siga o guia de instalação do dia em que você for instalar. A configuração muda entre versões. Um trecho copiado de blog antigo é a forma mais comum de o Vite subir sem estilo nenhum.

## Tarefas

Trabalhe no `estante-react`, numa branch `tailwind`, para o CSS antigo continuar no histórico.

### Tarefa 1 — O que a classe substitui

**Faça:** escolha a ficha do item. Copie para o README três regras do seu CSS (margem, cor do estado, layout) e, ao lado, a classe que você acha que corresponde, ainda sem instalar. Você confere na tarefa 4.

**Pronto quando:** os três pares estão escritos.

### Tarefa 2 — Instalar no Vite que já existe

**Leia:** [instalação com Vite](https://tailwindcss.com/docs/installation/using-vite) do começo ao fim, inclusive o jeito de carregar o Tailwind no CSS.

**Faça:** siga o guia no `estante-react`. Apague um pedaço pequeno do CSS antigo da página inicial e ponha classes no `h1` (tamanho, peso, cor). Recarregue.

**Pronto quando:** o título muda de verdade com as classes, e `npm run dev` continua de pé. Se o estilo não aparecer, o erro está na instalação, não na classe: volte ao guia antes de empilhar configuração.

### Tarefa 3 — A página sem o CSS antigo

**Faça:** tire o arquivo de CSS da sessão 13 do import principal, ou esvazie-o, e deixe a tela "crua" por um momento. Anote o que quebrou. Isso evita ficar com duas fontes de verdade.

**Pronto quando:** o que você vê vem do Tailwind. Um print mental ou uma frase no README descreve a tela antes de reestilizar.

### Tarefa 4 — Traduzir a ficha

**Leia:** espaçamento, cor, tipografia e flexbox no índice da documentação. Use a busca do site para o nome da propriedade que você já conhece (`padding`, `display: flex`, `gap`).

**Faça:** `ItemCard` volta a parecer uma ficha: borda ou fundo, espaço interno, título e estado alinhados. O estado não depende só da cor: o texto continua lá, como na sessão 03.

**Pronto quando:** três fichas lado a lado numa tela larga, com `gap`, e a classe que você chutou na tarefa 1 foi corrigida se estava errada.

### Tarefa 5 — Mobile primeiro

**Leia:** [responsive design](https://tailwindcss.com/docs/responsive-design).

**Faça:** a base, sem prefixo, é a tela estreita (uma coluna). A partir de `sm` ou `md`, a lista vira grid de três colunas. Confira em 360px e em 1200px.

**Pronto quando:** não há barra horizontal, e o prefixo está na regra da tela larga, não o contrário.

### Tarefa 6 — Formulário e foco

**Leia:** [hover, focus and other states](https://tailwindcss.com/docs/hover-focus-and-other-states), em especial `focus-visible`.

**Faça:** campos com rótulo, borda, e um anel ou contorno quando o teclado chega neles. Botão com estado de desabilitado (`disabled:opacity-50` ou o equivalente da versão que você instalou) enquanto o POST viaja.

**Pronto quando:** dá para incluir um item só com Tab e Enter, e você vê onde está o foco.

### Tarefa 7 — Repetição que merece nome

**Leia:** [reusing styles](https://tailwindcss.com/docs/reusing-styles).

**Faça:** se a mesma sequência longa de classes aparece em três lugares, extraia um componente React (você já sabe fazer isso) em vez de copiar a string. Use `@apply` só se a documentação da sua versão ainda recomendar e o caso for pequeno. Escreva no README qual caminho você escolheu.

**Pronto quando:** a ficha e o botão principal não são um copia-e-cola de quarenta classes.

### Tarefa 8 — O que ficou de fora

**Faça:** uma coisa do visual antigo que o vocabulário padrão não expressou bem (uma fonte específica, um detalhe de marca). Resolva com a forma que a documentação indicar para valor arbitrário ou para CSS seu, e diga por que não valia a pena forçar.

**Pronto quando:** o README tem esse caso, mesmo que a decisão tenha sido "não precisei".

### Tarefa 9 — Build

**Faça:** `npm run build`. O CSS gerado não precisa ser lido inteiro. Confirme que a tela de preview continua igual à de desenvolvimento.

**Pronto quando:** `npm run preview` mostra a Estante estilizada falando com a API.

### Tarefa 10 — Comparar

**Faça:** cinco linhas no README. O que ficou mais rápido do que o CSS à mão. O que ficou mais difícil de achar. O que você não quer perder da sessão 03 (hierarquia, contraste, foco).

**Pronto quando:** as cinco linhas existem e citam uma classe real do seu código.

## Checklist

- [ ] 1. Três regras antigas traduzidas no chute
- [ ] 2. Tailwind instalado pelo guia do Vite
- [ ] 3. CSS antigo fora do caminho
- [ ] 4. Ficha refeita
- [ ] 5. Uma coluna no celular, três na tela larga
- [ ] 6. Foco visível e botão desabilitado
- [ ] 7. Repetição extraída com critério
- [ ] 8. Um caso que não cabia na classe pronta
- [ ] 9. Build e preview
- [ ] 10. Comparação escrita

## Próximo passo

Siga para [15 · TypeScript com Node](../15-typescript-com-node/README.md). O React fica em JavaScript de propósito: o tipo entra primeiro no servidor, onde a regra de negócio mora. O visual desta sessão volta no Next.js.
