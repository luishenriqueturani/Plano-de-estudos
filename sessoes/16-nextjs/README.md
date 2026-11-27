# Sessão 16 — Next.js, TypeScript e SSR

**Sessão 16 de 27 · cerca de 6 semanas**

O Next.js junta a interface React e um servidor de páginas. Com SSR, o HTML da lista já chega montado: quem abre a Estante recebe os itens no primeiro carregamento, e o navegador não precisa esperar um `useEffect` para ver alguma coisa.

Nesta sessão o projeto é fullstack dentro do Next: as páginas e o acesso ao Postgres moram no mesmo repositório. Na fase do Nest, o acesso ao banco sai daqui e fica na API. Você vai sentir a diferença porque viveu os dois formatos.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [15 · TypeScript com Node](../15-typescript-com-node/README.md)
> **Próxima →** [17 · NestJS](../17-nestjs/README.md)

## O que você vai saber fazer

- Criar um app com App Router e TypeScript.
- Distinguir componente de servidor e componente de cliente.
- Buscar os itens no servidor e renderizar HTML já preenchido.
- Gravar dados com Server Actions ou Route Handlers, com validação no servidor.
- Separar o que pode ir para o navegador do que fica só no servidor (senha do banco).

## Documentação

- [Documentação do Next.js](https://nextjs.org/docs)
- [Getting started do App Router](https://nextjs.org/docs/app/getting-started)
- [Instalação](https://nextjs.org/docs/app/getting-started/installation)
- [Layouts e páginas](https://nextjs.org/docs/app/getting-started/layouts-and-pages)
- [Busca de dados](https://nextjs.org/docs/app/getting-started/fetching-data)
- [Mutações](https://nextjs.org/docs/app/getting-started/mutating-data)
- [Server e Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)
- [Route Handlers](https://nextjs.org/docs/app/getting-started/route-handlers)
- [Variáveis de ambiente](https://nextjs.org/docs/app/guides/environment-variables)
- [TypeScript no Next](https://nextjs.org/docs/app/api-reference/config/typescript)
- [Segurança de dados](https://nextjs.org/docs/app/guides/data-security)

Se um desses caminhos tiver sido renomeado, entre em [nextjs.org/docs](https://nextjs.org/docs) e busque o título. Não siga tutorial de Pages Router (`pages/`): este plano usa App Router (`app/`).

## Tarefas

Repositório `estante-next`. Postgres da sessão 10, esquema já criado. Pode reutilizar o SQL. Não precisa da API da sessão 12 rodando: o Next fala com o banco nesta fase.

### Tarefa 1 — Onde o HTML nasce

**Leia:** [server and client components](https://nextjs.org/docs/app/getting-started/server-and-client-components).

**Faça:** no README, explique SSR com a Estante: o que já vem no HTML, o que ainda precisa de JavaScript no navegador (filtro digitado, por exemplo). Duas listas curtas.

**Pronto quando:** as listas existem antes de criar o app.

### Tarefa 2 — Criar o projeto

**Leia:** [instalação](https://nextjs.org/docs/app/getting-started/installation).

**Faça:** `create-next-app` com TypeScript, App Router, sem misturar exemplo demais. ESLint pode ficar ligado. Entre na pasta, `npm run dev`, apague o conteúdo de marketing da página inicial e deixe o nome da Estante.

**Pronto quando:** a página inicial é sua e o `tsconfig.json` existe.

### Tarefa 3 — Layout

**Leia:** [layouts and pages](https://nextjs.org/docs/app/getting-started/layouts-and-pages).

**Faça:** `app/layout.tsx` com cabeçalho, navegação (Início, Sobre) e rodapé. `app/page.tsx` e `app/sobre/page.tsx`. O título do documento usa a API de metadata da documentação.

**Pronto quando:** as duas rotas funcionam e o cabeçalho não foi copiado na mão dentro de cada página.

### Tarefa 4 — Ler o banco no servidor

**Leia:** [fetching data](https://nextjs.org/docs/app/getting-started/fetching-data) e a parte de pacotes no servidor. Para o Postgres, use `pg` num módulo `src/db.ts` importado só por código de servidor.

**Faça:** a página inicial consulta os itens e renderiza a lista. Não use `useEffect` para essa carga.

**Pronto quando:** "ver código-fonte" no navegador já contém os títulos, antes de qualquer hidratação. Se não contiver, a busca está no cliente e precisa mudar de lugar.

### Tarefa 5 — Página dinâmica

**Leia:** a parte de rotas dinâmicas na documentação de layouts e páginas.

**Faça:** `app/itens/[id]/page.tsx` mostra o item e as notas. Id que não existe chama `notFound()`.

**Pronto quando:** um id inválido mostra a tela de não encontrado, não um stacktrace.

### Tarefa 6 — Variáveis de ambiente

**Leia:** [environment variables](https://nextjs.org/docs/app/guides/environment-variables).

**Faça:** a string do banco é `DATABASE_URL`, sem prefixo `NEXT_PUBLIC_`. Crie uma variável pública só para um texto inofensivo (o nome da coleção) e use no cliente, para ver a diferença. Tente, de propósito, ler `DATABASE_URL` num componente com `'use client'` e registre o que o Next faz. Desfaça a tentativa.

**Pronto quando:** o README explica por que a URL do banco não leva `NEXT_PUBLIC_`, com o que você observou.

### Tarefa 7 — Criar item no servidor

**Leia:** [mutating data](https://nextjs.org/docs/app/getting-started/mutating-data).

**Faça:** um formulário cria item com Server Action. Valide título vazio no servidor. No sucesso, a lista atualiza (`revalidatePath` ou o mecanismo que a página atual da documentação indicar). Mostre o erro de validação ao lado do campo.

**Pronto quando:** criar funciona com JavaScript do navegador desligado, se a documentação do formulário que você usou permitir envio nativo. Se o seu formulário depender de cliente, anote e mantenha a validação no servidor do mesmo jeito: teste a action com um teste ou com uma chamada direta.

### Tarefa 8 — Avançar, editar, apagar

**Faça:** actions ou route handlers para os três. Confirmação de apagar pode ser um formulário. A regra do estado continua no servidor, e o `CHECK` do banco continua valendo.

**Pronto quando:** os três persistem no `psql`.

### Tarefa 9 — Notas

**Faça:** o formulário na página do item grava uma nota e a página mostra a lista atualizada.

**Pronto quando:** duas notas no mesmo item aparecem em ordem definida (a mais nova primeiro, ou a mais antiga: escolha e mostre).

### Tarefa 10 — Um pedaço cliente

**Faça:** a busca por texto, se você quiser resposta enquanto digita, fica num client component que recebe a lista já carregada ou busca uma rota. Filtro de estado pode ser um link com query (`/?estado=fazendo`) resolvida no servidor, o que combina mais com SSR. Faça o filtro por link e a busca digitada no cliente, e explique os dois no README.

**Pronto quando:** recarregar `/?estado=fazendo` já traz só aquele estado no HTML.

### Tarefa 11 — Route Handler de leitura

**Leia:** [route handlers](https://nextjs.org/docs/app/getting-started/route-handlers).

**Faça:** `GET /api/itens` devolve JSON. É o embrião da API que o Nest vai substituir. A página principal não precisa usar essa rota: ela continua lendo no servidor. A rota existe para você ver as duas portas de saída (HTML e JSON).

**Pronto quando:** `curl` no endpoint devolve os mesmos itens da página.

### Tarefa 12 — Erro e carregamento

**Leia:** as convenções `loading.tsx` e `error.tsx` na documentação do App Router.

**Faça:** `loading.tsx` na lista. `error.tsx` com uma mensagem sua. Force um erro (banco desligado) e veja a tela.

**Pronto quando:** banco desligado não mostra o stacktrace padrão para quem usa a página. O log do servidor ainda tem o erro técnico.

### Tarefa 13 — Segurança de dados, primeira passagem

**Leia:** [data security](https://nextjs.org/docs/app/guides/data-security).

**Faça:** no README, responda:

- a Server Action confere a entrada, ou confia no formulário?
- algum segredo foi parar em componente cliente?
- a página de detalhe vaza campo que não deveria (hash no futuro, por exemplo)? Hoje, confirme que você só manda título, ano, estado, notas.

**Pronto quando:** as três respostas citam arquivo.

### Tarefa 14 — CSS

**Faça:** use Tailwind, como na sessão 14. Traga a identidade visual das sessões anteriores. Layout estável em 360px. CSS module fica só se uma parte não couber em classe utilitária, e o README diz qual.

**Pronto quando:** a lista e o detalhe parecem o mesmo produto de antes.

### Tarefa 15 — Build de produção

**Faça:** `npm run build` e `npm run start`. Corrija o que o build reclamar. Anote o que o build prova que o `dev` não prova (tipagem, rotas, variáveis).

**Pronto quando:** `next start` serve a lista vinda do banco.

### Tarefa 16 — README

**Faça:** requisitos, `.env.example`, Compose, `npm install`, `npm run dev`, `npm run build`. Diga que esta versão fala com o Postgres direto e que a sessão 23 troca isso pela API do Nest.

**Pronto quando:** a frase sobre a sessão 23 está escrita. Ela evita que você se perca quando o acesso ao banco mudar de lugar.

## Checklist

- [ ] 1. SSR explicado com a Estante
- [ ] 2. App criado em TypeScript
- [ ] 3. Layout compartilhado
- [ ] 4. Títulos presentes no código-fonte
- [ ] 5. `notFound()` no id inválido
- [ ] 6. Segredo fora de `NEXT_PUBLIC_`
- [ ] 7. Criar no servidor
- [ ] 8. Avançar, editar, apagar
- [ ] 9. Notas ordenadas
- [ ] 10. Filtro na URL e busca no cliente
- [ ] 11. `GET /api/itens`
- [ ] 12. Erro amigável com o banco desligado
- [ ] 13. Três respostas de segurança
- [ ] 14. Visual nos dois tamanhos
- [ ] 15. `build` e `start`
- [ ] 16. README com o aviso da sessão 23

## Próximo passo

Siga para [17 · NestJS](../17-nestjs/README.md).
