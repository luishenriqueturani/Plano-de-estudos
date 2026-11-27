# Sessão 15 — TypeScript com Node

**Sessão 15 de 27 · cerca de 4 semanas**

O JavaScript aceita passar um item sem título e só descobre na hora. O TypeScript aponta isso antes de rodar, se os tipos forem honestos. Esta sessão tipa a regra da Estante e um pedaço do servidor. Next.js e NestJS, nas sessões seguintes, já nascem em TypeScript. Quem chega lá sem ter lutado com um erro de tipo sofre nas duas ao mesmo tempo.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [14 · Tailwind](../14-tailwind/README.md)
> **Próxima →** [16 · Next.js](../16-nextjs/README.md)

## O que você vai saber fazer

- Ligar o compilador em modo estrito.
- Modelar o item com tipos, uniões e funções genéricas simples.
- Corrigir erros lendo a mensagem, não silenciando com `any`.
- Rodar o servidor em TypeScript durante o desenvolvimento e gerar JavaScript para executar.

## Documentação

- [Handbook, introdução](https://www.typescriptlang.org/docs/handbook/intro.html)
- [Tipos do dia a dia](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [Estreitando tipos](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [Objetos](https://www.typescriptlang.org/docs/handbook/2/objects.html)
- [Funções](https://www.typescriptlang.org/docs/handbook/2/functions.html)
- [Unions e tipos literais](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) — a página de everyday types cobre o essencial; aprofunde em [creating types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) se precisar
- [tsconfig](https://www.typescriptlang.org/tsconfig/)
- [Módulos ESM no Node com TypeScript](https://nodejs.org/api/typescript.html) — leia também o que a sua versão do Node já executa sem ferramenta extra

## Tarefas

Repositório `estante-tipos`, ou uma branch da API. Mantenha o comportamento. A mudança é o tipo e a organização.

### Tarefa 1 — Um erro que o JavaScript deixou passar

**Faça:** lembre, ou provoque, um bug de campo com nome errado (`item.titulo` escrito `item.titlo`) que só apareceu ao rodar. Descreva no README. Esse bug é o motivo da sessão.

**Pronto quando:** a história está escrita, com o que você viu na tela ou no log.

### Tarefa 2 — Compilador

**Faça:**

```bash
npm install -D typescript
npx tsc --init
```

Em `tsconfig.json`, ligue `"strict": true`. Defina o que mais a documentação de [tsconfig](https://www.typescriptlang.org/tsconfig/) pedir para um projeto Node ESM moderno (`module`, `moduleResolution`, `target`, `outDir`, `rootDir`). Anote cada opção que você mudou e a razão, numa linha.

**Pronto quando:** `npx tsc --noEmit` roda. Pode falhar: a tarefa 3 começa a corrigir. O comando em si precisa existir.

### Tarefa 3 — O modelo

**Leia:** [everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html).

**Faça:** em `src/tipos.ts`:

```ts
export type Estado = 'quero' | 'fazendo' | 'feito';

export interface Item {
  id: number;
  titulo: string;
  ano: number | null;
  estado: Estado;
  criadoEm: string;
}
```

Ajuste os nomes aos que o seu banco já usa e faça a tradução num único lugar (função `linhaParaItem`), se a coluna for `criado_em` e o objeto for `criadoEm`.

**Pronto quando:** uma função que recebe `Estado` recusa a string `'talvez'` na checagem do `tsc`, e você viu essa recusa.

### Tarefa 4 — Funções da regra

**Leia:** [funções](https://www.typescriptlang.org/docs/handbook/2/functions.html).

**Faça:** traga `avancarEstado`, `criarItem` e `filtrarPorEstado` para `.ts`, com tipos nos parâmetros e no retorno. O `strict` já liga `noImplicitAny`. Não anote `any` para calar o compilador. Para executar o teste, use o modo de TypeScript que a [documentação do Node](https://nodejs.org/api/typescript.html) da sua versão permitir, ou instale `tsx` e registre o script. Siga a página oficial da ferramenta que escolher.

**Pronto quando:** `npm test` passa e um retorno errado (devolver string onde é `Item`) falha no `tsc`.

### Tarefa 5 — Estreitar

**Leia:** [narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html).

**Faça:** a função que interpreta o JSON do `POST` recebe `unknown`, não `any`. Ela confere se é objeto, se `titulo` é string não vazia e se `ano` é número ou está ausente. O retorno é um tipo `NovoItem`. Fora isso, um erro seu (`ErroDeValidacao`) carrega a mensagem para a rota transformar em 400.

**Pronto quando:** existe teste para corpo que não é objeto, título numérico e ano ausente.

### Tarefa 6 — A rota usa o tipo

**Faça:** converta o roteador da sessão 11/10 para TypeScript. O lugar que monta a resposta 400 usa o erro da tarefa 5. O lugar que fala com o `pg` tipa a linha que volta, ou passa pela função `linhaParaItem` antes de chegar na rota.

**Pronto quando:** `titulo` com grafia errada não compila, e a API continua respondendo no `curl`.

### Tarefa 7 — União em vez de enum

**Faça:** mantenha `Estado` como união de literais. Se você criou um `enum`, troque e anote a razão: a união some no JavaScript gerado e casa direto com o `CHECK` do banco. Leia a página de enums do handbook só o bastante para saber que ela existe e por que você não usou.

**Pronto quando:** o README tem essas três linhas.

### Tarefa 8 — Null de propósito

**Faça:** decida se `ano` pode ser `null`. O tipo, o SQL e o formulário (quando você voltar no React) precisam concordar. Escreva a decisão. Ajuste uma função que hoje estoura ao ler `item.ano.toString()` sem checar.

**Pronto quando:** o `tsc` exige a checagem, e você a escreveu.

### Tarefa 9 — Configuração tipada

**Faça:** um módulo `src/config.ts` lê `process.env`, confere `PORT` e a URL do banco, e exporta um objeto já convertido (`port` como número). Se faltar variável, o processo termina na subida com a lista do que falta.

**Pronto quando:** apagar uma chave do `.env` impede o servidor de subir, com a sua mensagem.

### Tarefa 10 — Scripts

**Faça:**

- `npm run typecheck` → `tsc --noEmit`
- `npm run dev` → o executável que você escolheu na tarefa 4
- `npm start` → `tsc` e depois `node` na pasta de saída, ou o modo que a documentação do Node indicar para rodar TypeScript

**Pronto quando:** os três scripts estão no README e você rodou cada um.

### Tarefa 11 — O que não tipar com mentira

**Faça:** procure `any` e `as` no projeto. Cada `as` que sobrar precisa de um comentário de uma linha dizendo por que a checagem automática não deu conta. Zero `any` é a meta. Se sobrar um, ele é local e justificado.

**Pronto quando:** `grep` de `any` no `src/` não mostra atalho preguiçoso. O `grep` pode achar a palavra dentro de comentário: leia o contexto.

### Tarefa 12 — README

**Faça:** como rodar, a versão do TypeScript, e dois erros reais que o compilador pegou, com a mensagem resumida.

**Pronto quando:** esses dois erros estão descritos de um jeito que você entenderia daqui a um mês.

## Checklist

- [ ] 1. Bug de campo narrado
- [ ] 2. `strict` ligado
- [ ] 3. `Estado` e `Item`
- [ ] 4. Regras com teste em TypeScript
- [ ] 5. JSON entra como `unknown`
- [ ] 6. API compilando e respondendo
- [ ] 7. União no lugar de enum
- [ ] 8. `null` tratado
- [ ] 9. Config que falha cedo
- [ ] 10. `typecheck`, `dev` e `start`
- [ ] 11. `any` revisado
- [ ] 12. Dois erros reais no README

## Próximo passo

Siga para [16 · Next.js](../16-nextjs/README.md).
