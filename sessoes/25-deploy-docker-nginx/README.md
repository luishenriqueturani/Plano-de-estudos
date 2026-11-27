# Sessão 25 — Deploy com Docker e Nginx

**Sessão 25 de 27 · cerca de 3 semanas**

Até agora cada coisa sobe na mão: Compose do banco, `npm run dev` do Nest, `npm run dev` do Next. Esta sessão faz a mesma Estante subir com um comando, atrás do Nginx. O Nginx é a única porta publicada. O Postgres não aparece na rede da sua máquina para o mundo, nem para a sua rede de casa além do necessário: a API fala com ele pela rede interna do Compose.

O alvo é o seu computador, imitando um servidor. Colocar isso numa VPS vem depois do checklist, com HTTPS. Não abra a porta do banco para a internet para "testar de fora".

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [24 · E-mail e push](../24-email-e-push/README.md)
> **Próxima →** [26 · Observabilidade](../26-observabilidade/README.md)

## O que você vai saber fazer

- Escrever Dockerfile de produção do Nest e do Next, com build separado da imagem final.
- Subir Postgres, API, site e Nginx num `compose.yaml`.
- Encaminhar `/api` para o Nest e o resto para o Next.
- Reiniciar sem perder dado, e saber onde está o log quando a página não abre.

## Documentação

- [Multi-stage build](https://docs.docker.com/build/building/multi-stage/)
- [Compose](https://docs.docker.com/compose/gettingstarted/)
- [Guia inicial do Nginx](https://nginx.org/en/docs/beginners_guide.html)
- [Proxy reverso no Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)
- [Imagem oficial do Nginx](https://hub.docker.com/_/nginx)
- [Imagem oficial do Postgres](https://hub.docker.com/_/postgres), de novo, a parte de volume e variáveis
- [Docker no Arch](https://wiki.archlinux.org/title/Docker)
- [Variáveis de ambiente do Next em produção](https://nextjs.org/docs/app/guides/environment-variables)
- A documentação de `output: 'standalone'` do Next, buscada em [nextjs.org/docs](https://nextjs.org/docs) por "standalone", para a imagem ficar menor
- O checklist de produção que você começou na [sessão 22](../22-seguranca-backend/README.md)

## Tarefas

Pode ser um repositório `estante-deploy` que referencia os outros, ou uma pasta `deploy/` no repositório que você escolher como guarda-chuva. Os Dockerfiles ficam junto de cada app. O Compose pode ficar no guarda-chuva e construir com `context` apontando para cada pasta. Documente o layout.

### Tarefa 1 — O que muda do dev para essa imagem

**Faça:** no README, uma tabela: comando de desenvolvimento, comando de produção, onde mora a variável, se o código-fonte está dentro do container. A imagem de produção não roda `nest start --watch` nem `next dev`.

**Pronto quando:** a tabela existe e você ainda não escreveu o Dockerfile. Ela é o critério das tarefas seguintes.

### Tarefa 2 — Dockerfile do Nest

**Leia:** [multi-stage](https://docs.docker.com/build/building/multi-stage/).

**Faça:** um estágio instala dependência e compila. O estágio final leva só o que precisa para `node dist/main.js` (ou o caminho real do seu build), com a versão LTS que o `.nvmrc` pede, fixada na tag da imagem (`node:22` não, uma tag mais específica que você copiar do Docker Hub no dia, como `node:22.x.x`). `.dockerignore` exclui `node_modules`, `.env`, `dist` local e `.git`.

**Pronto quando:** `docker build` termina e um `docker run` com as variáveis de exemplo sobe até falhar no banco ausente, com a sua mensagem de configuração, não com "arquivo não encontrado".

### Tarefa 3 — Dockerfile do Next

**Leia:** a página de standalone no docs do Next.

**Faça:** build de produção dentro da imagem e processo final enxuto, do jeito que essa página indicar. Variáveis públicas, se ainda existir alguma inofensiva, entram no build como a documentação manda. Segredo não entra como `ARG` que fique na camada final. Confira o histórico da imagem (`docker history`) procurando a senha de teste.

**Pronto quando:** a imagem sobe e a senha não aparece no histórico.

### Tarefa 4 — Compose com quatro serviços

**Faça:** `postgres`, `api`, `web`, `nginx`.

- Postgres usa volume nomeado e não publica porta para o host. A API usa o nome do serviço como host (`postgres`), pela rede do Compose.
- API e web também ficam sem porta publicada. Só o Nginx publica `8080:80` nesta etapa. 80 no host costuma exigir privilégio; 8080 chega para o estudo.
- `depends_on` não espera o Postgres estar pronto para conexão. Use o healthcheck da imagem, se a documentação do Compose e do Postgres que você ler mostrar como, ou um retry na API. Anote o que você fez, porque "depends_on sozinho" não basta e você provavelmente vai ver a API reiniciar uma vez.

**Pronto quando:** `docker compose up --build` sobe os quatro e `docker compose ps` mostra o que está saudável.

### Tarefa 5 — Nginx na frente

**Leia:** [beginners guide](https://nginx.org/en/docs/beginners_guide.html) e [reverse proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/).

**Faça:** um arquivo de configuração montado no container:

- `/api/` vai para o serviço da API. Decida se o Nest tem prefixo global `api` ou se o Nginx remove o prefixo, e escreva a decisão. As duas funcionam se o `fetch` do Next usar a mesma URL que o navegador vê.
- o restante vai para o Next.
- o Nginx não serve o diretório do código. Ele só encaminha.
- um `location` de saúde pode responder texto simples, sem chegar no Nest, para você saber que o proxy está vivo.

**Pronto quando:** `http://localhost:8080` abre a Estante e `http://localhost:8080/api/...` chega no Nest. Você confirma nos logs de qual container.

### Tarefa 6 — Configuração de produção local

**Faça:** `.env` fora da imagem, passado pelo Compose. `.env.example` sem senha real. `NODE_ENV=production` na API, para o Nest recusar store de autenticação só em memória, como o checklist da sessão 22. As migrations rodam como um comando explícito (`docker compose run api npm run migration:run`), não escondidas num jeito que aplique de novo a cada request. O seed de categorias, idem.

**Pronto quando:** uma base vazia, seguindo o README, recebe migration, seed e login pela interface em `localhost:8080`.

### Tarefa 7 — Dado que sobrevive

**Faça:** crie um item, `docker compose down` (sem `-v`), `docker compose up -d`, entre de novo. O item continua. Depois leia o que `-v` apaga e não use no volume bom.

**Pronto quando:** você fez esse ciclo e o README avisa sobre o `-v`.

### Tarefa 8 — Logs

**Faça:** provoque um erro (senha do banco errada num experimento, depois desfaça) e ache a causa com `docker compose logs api`. Provoque um proxy errado (upstream com nome trocado) e ache no log do Nginx. Volte a configuração boa.

**Pronto quando:** o README tem os dois comandos de log que você realmente usou.

### Tarefa 9 — O que não ficou exposto

**Faça:** com a stack no ar, confira:

- `ss` ou `docker compose ps` mostram publicado só o Nginx;
- a porta 5432 não está no host;
- a documentação do Compose não está com `ports: "5432:5432"` comentada "por enquanto" e esquecida.

**Pronto quando:** essa conferência está escrita, com a saída resumida.

### Tarefa 10 — Recurso e reinício

**Faça:** `restart: unless-stopped` nos serviços que devem voltar se o Docker subir de novo, como a documentação do Compose descrever. Reinicie o serviço do Docker na sua máquina (ou só os containers) e veja a Estante voltar. Se reiniciar o serviço do sistema for invasivo demais, reinicie só os containers e anote que o boot da máquina ficou por sua conta.

**Pronto quando:** o comportamento que você testou está no README, sem prometer o que você não testou.

### Tarefa 11 — Checklist antes de um servidor de verdade

**Faça:** uma lista, ainda sem contratar máquina:

- HTTPS na frente (a documentação de TLS do Nginx e, quando for a hora, o cliente que a wiki do Arch indicar para certificado). Enquanto for HTTP, o cookie `Secure` da sessão pode impedir o login fora de localhost: anote o que a sua stack fez em `http://localhost:8080` e o que precisará mudar com HTTPS;
- backup do volume testado com `pg_dump`, como na sessão 10, agora via `docker compose exec`;
- imagem fixada por versão, não por `latest`;
- segredo fora do Git e fora do `docker history`;
- porta do banco fechada.

**Pronto quando:** cada item está "feito aqui" ou "só quando houver servidor", sem meio-termo.

### Tarefa 12 — README que sobe a stack

**Faça:** do zero, numa explicação curta: requisitos (Docker), copiar `.env`, build, migration, seed, URL, como parar, como ver log, como não apagar o volume.

**Pronto quando:** você segue o README depois de `docker compose down` e chega na tela de login.

## Checklist

- [ ] 1. Tabela dev versus produção
- [ ] 2. Imagem do Nest
- [ ] 3. Imagem do Next sem segredo no histórico
- [ ] 4. Quatro serviços, banco sem porta publicada
- [ ] 5. Nginx encaminha site e API
- [ ] 6. Migration explícita e login pela porta 8080
- [ ] 7. Volume sobrevive ao `down`
- [ ] 8. Logs da API e do Nginx usados de verdade
- [ ] 9. Só o Nginx está publicado
- [ ] 10. Reinício testado e descrito com precisão
- [ ] 11. Checklist do que falta para um servidor
- [ ] 12. README seguido do zero

## Próximo passo

Siga para [26 · Observabilidade](../26-observabilidade/README.md). A stack sobe. A próxima sessão mostra se ela está saudável.
