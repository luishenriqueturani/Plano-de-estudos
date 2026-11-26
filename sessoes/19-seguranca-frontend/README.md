# Sessão 19 — Segurança do frontend com Next e Nest

**Sessão 19 de 21 · cerca de 3 semanas**

O Next.js deixa de falar direto com o Postgres. Quem manda no dado é o Nest. O navegador entra na conta, vê só a própria estante e não guarda access token em `localStorage`. O cookie de sessão fica `HttpOnly`: o JavaScript da página não lê esse valor. O JWT da sessão 18 continua válido para o `curl` e para um cliente que não é o site.

Esconder botão não protege nada. A API já recusa. O frontend só precisa não mentir para a pessoa, não vazar segredo e não guardar credencial onde um script da página alcança.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [18 · Segurança do backend](../18-seguranca-backend/README.md)
> **Próxima →** [20 · Deploy com Docker e Nginx](../20-deploy-docker-nginx/README.md)

## O que você vai saber fazer

- Ligar o Next à API do Nest, com a origem combinada no CORS.
- Manter a sessão no cookie que o Nest define, com os atributos que a documentação pede.
- Tratar 401 na interface sem apagar a diferença entre "não logado" e "proibido".
- Evitar os vazamentos comuns de frontend: segredo em variável pública, HTML cru com texto da pessoa, token no `localStorage`.

## Documentação

- [Autenticação no Next.js](https://nextjs.org/docs/app/guides/authentication)
- [Segurança de dados no Next.js](https://nextjs.org/docs/app/guides/data-security)
- [Variáveis de ambiente](https://nextjs.org/docs/app/guides/environment-variables)
- [Cookies na MDN](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Guides/Cookies)
- [CORS no Nest](https://docs.nestjs.com/security/cors)
- A seção de cookie de sessão e a de CSRF em [Authentication](https://docs.nestjs.com/security/authentication) (o checklist de produção cita `enableCsrfProtection`)
- [Prevenção de XSS](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) — as regras de saída; não a parte de como injetar
- [Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

## Tarefas

Volte ao repositório `estante-next`. Remova o acesso direto ao Postgres das páginas. Se quiser preservar a versão da sessão 13, faça isso numa branch `api-nest` e deixe a outra branch no histórico.

### Tarefa 1 — O navegador não é barreira

**Leia:** o começo de [data security](https://nextjs.org/docs/app/guides/data-security) e de [authentication](https://nextjs.org/docs/app/guides/authentication).

**Faça:** no README, três frases:

- o que o frontend garante para a experiência (não mostrar a tela de outra pessoa, não pedir de novo o que já se sabe);
- o que só o Nest garante (o dado não sai, a senha não volta, o item alheio não muda);
- por que um `if` no React que esconde o botão "apagar" não substitui o teste da sessão 18.

**Pronto quando:** as três frases estão no topo do README.

### Tarefa 2 — Origem combinada

**Faça:** decida as portas em desenvolvimento (por exemplo Next em 3000 e Nest em 3001, ou o par que você já usa). O CORS do Nest lista exatamente a origem do Next, com credencial se o cookie precisar. A URL da API no Next é variável de servidor, sem `NEXT_PUBLIC_`, quando quem chama o Nest é o servidor do Next. Se algum `fetch` for do navegador direto para o Nest, aí a URL precisa ser pública: escreva qual dos dois modelos você usou e por quê.

**Pronto quando:** um pedido real funciona e uma origem errada continua barrada, como na tarefa 10 da sessão 18.

### Tarefa 3 — Login e cadastro

**Leia:** a parte de formulário de credenciais no guia de autenticação do Next, e a seção de cookie de sessão no guia do Nest.

**Faça:** telas de cadastro e de entrar. A senha vai no corpo do POST para o Nest, em HTTPS no futuro e em localhost agora. Não grave a senha em estado global, log ou query string. Sucesso leva à lista. Falha mostra uma mensagem única, a mesma para e-mail desconhecido e senha errada, se o Nest já responder assim.

**Pronto quando:** o cookie aparece no painel de armazenamento do navegador com `HttpOnly`, e o JavaScript da página não tem um `localStorage.setItem` de token. Procure no projeto por `localStorage` e por `sessionStorage`.

### Tarefa 4 — A lista vem do Nest

**Faça:** a página inicial, no servidor, busca os itens com a credencial da pessoa (o cookie encaminhado como a documentação do Nest e a do Next descreverem para o seu modelo). Sem sessão, redireciona para entrar. O HTML inicial contém os títulos dela, como no SSR da sessão 13.

**Pronto quando:** "ver código-fonte" mostra os itens depois do login, e não mostra esses itens numa janela anônima.

### Tarefa 5 — Criar, editar, apagar, notas

**Faça:** as mutações chamam o Nest e incluem a credencial. Erro 400 aparece no formulário. Erro 401 manda para o login. Erro 403 ou 404 do item alheio mostra uma página sóbria, sem o corpo interno do Nest se ele tiver detalhe demais (ajuste o Nest se tiver).

**Pronto quando:** os fluxos da Estante funcionam de novo, agora com dois usuários no mesmo Postgres enxergando coleções diferentes.

### Tarefa 6 — Sair

**Faça:** sair chama o endpoint de encerrar sessão que a documentação do Nest fornecer, e o Next manda a pessoa para a tela de entrar. Voltar no botão do navegador não mostra a lista como se ainda estivesse logada: a próxima navegação busca de novo e cai no login.

**Pronto quando:** você descreve o que fez para confirmar isso, com os cliques reais.

### Tarefa 7 — CSRF, porque o cookie vai sozinho

**Leia:** o item de `enableCsrfProtection` no checklist de produção do Nest, e a página de CSRF da documentação se ela existir com esse nome (busque "CSRF" em [docs.nestjs.com](https://docs.nestjs.com/)). Leia também o que o guia do Next diz sobre ações no servidor.

**Faça:** ligue a proteção do jeito que a sua versão do Nest pede, com a origem do Next na lista de origens confiáveis. Uma escrita vinda de outra origem falha. A escrita vinda do seu formulário continua funcionando.

**Pronto quando:** os dois casos estão anotados com o status observado.

### Tarefa 8 — Segredo não vai para o bundle

**Leia:** [environment variables](https://nextjs.org/docs/app/guides/environment-variables).

**Faça:** confirme que `DATABASE_URL` saiu do Next, se ainda estava lá. Confirme que `JWT_SECRET` não existe neste repositório. Nenhuma variável `NEXT_PUBLIC_` carrega senha, token ou URL com usuário de banco. Rode o build e procure, na pasta de saída, um pedaço da senha de teste. Não deve estar lá.

**Pronto quando:** a busca na pasta de build não acha a senha, e o README lista as variáveis que o Next ainda tem.

### Tarefa 9 — Texto da pessoa é texto

**Leia:** a cheat sheet de prevenção de XSS, a regra de não inserir HTML não confiável.

**Faça:** título e nota são renderizados como texto. Não use `dangerouslySetInnerHTML` com dado vindo da API. Se precisar de ênfase, o HTML é seu, o dado continua em texto. Tente gravar um título com `<b>oi</b>` e confirme que a página mostra os caracteres, sem virar negrito interpretado pelo navegador.

**Pronto quando:** esse título de teste aparece literal e o README conta o resultado.

### Tarefa 10 — O JWT não mora na página

**Faça:** o site não pede, não guarda e não reenvia o access token. O `curl` da sessão 18 continua sendo o cliente desse token. Escreva no README os dois clientes: navegador com cookie, terminal com Bearer.

**Pronto quando:** uma busca no repositório do Next por `Bearer` e por `JWT_SECRET` não mostra uso no app. Menção no README pode existir, desde que seja a explicação.

### Tarefa 11 — Duas contas, na mão

**Faça:** crie duas contas, um item em cada, e tente abrir no navegador o endereço do item da outra (o id na barra). A tela não mostra o conteúdo. Confira no Nest que a linha não mudou.

**Pronto quando:** você anota os dois ids e o que cada conta viu.

### Tarefa 12 — README de dois repositórios

**Faça:** a ordem para desenvolver: compose, migration, seed, Nest, Next. Variáveis de cada um. Como criar a primeira conta pela interface. O que é intencional: sem o Nest, o Next não tem dado.

**Pronto quando:** no dia seguinte você sobe os dois só com o README.

## Checklist

- [ ] 1. Três frases sobre quem protege o quê
- [ ] 2. CORS e o modelo de fetch decididos
- [ ] 3. Login com cookie `HttpOnly`, sem `localStorage`
- [ ] 4. Lista SSR só da conta logada
- [ ] 5. Mutações com 400, 401 e 403/404
- [ ] 6. Sair encerra a sessão de verdade
- [ ] 7. CSRF ligado e escrita legítima ok
- [ ] 8. Segredo fora do build
- [ ] 9. Título com tag aparece como texto
- [ ] 10. JWT fica no `curl`, não no site
- [ ] 11. Duas contas isoladas na interface
- [ ] 12. README dos dois processos

## Próximo passo

Siga para [20 · Deploy com Docker e Nginx](../20-deploy-docker-nginx/README.md).
