# Sessão 27 — Projeto final

**Sessão 27 de 27 · cerca de 6 semanas**

As sessões anteriores construíram a Estante de estudo. Esta é um produto com começo, meio e fim, no tema que você quiser. Pode continuar a Estante, se ela ainda te interessar, ou trocar o domínio (treinos, plantas, partidas, receitas). A troca de tema é livre. A troca de atalho não é: o que está na rubrica entra no projeto, mesmo que menor do que a Estante ficou.

O resultado é um repositório `estante-final` (ou o nome novo) que uma pessoa sua roda com o README e usa no navegador.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [26 · Observabilidade](../26-observabilidade/README.md)

Não há sessão seguinte. No fim, você escolhe o que aprofundar.

## O que você vai entregar

Uma aplicação com:

- conta, sessão no cookie e API que recusa o que a conta não pode;
- PostgreSQL, migration e seed;
- NestJS com módulo, validação, relação e uma rotina agendada que exista por um motivo do produto;
- Next.js com TypeScript, uma listagem renderizada no servidor e formulários que erram sem quebrar a tela;
- Docker Compose com Nginx na frente e o banco fora da porta publicada;
- README que sobe isso, e um texto curto de arquitetura.

## Documentação

A desta sessão é a que você já usou. Volte ao link específico quando a rubrica citar o assunto, em vez de reabrir o tutorial inteiro.

- [NestJS](https://docs.nestjs.com/)
- [Next.js](https://nextjs.org/docs)
- [TypeORM migrations](https://typeorm.io/docs/advanced-topics/migrations/)
- [Task scheduling](https://docs.nestjs.com/application/task-scheduling)
- [Authentication](https://docs.nestjs.com/security/authentication) e [Authorization](https://docs.nestjs.com/security/authorization)
- [Nginx reverse proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

## Rubrica

Cada linha é obrigatória. "Quase" não conta.

| Peça | Pronto quando |
| --- | --- |
| Escopo | Cabe numa página: o que o produto faz e três coisas que ele não faz |
| Modelo | Há mais de uma tabela, com chave estrangeira e uma regra que o banco também garante (`UNIQUE`, `CHECK` ou `NOT NULL`) |
| Migration | Banco vazio chega ao esquema só com `migration:run`, e `synchronize` está desligado |
| Seed | Dado de catálogo, idempotente, sem senha |
| API | CRUD do recurso principal no Nest, com DTO e teste do caso de dono ou de permissão |
| Auth | Cadastro, entrar, sair. Senha só como hash. Rota privada sem credencial responde 401 |
| Front | Lista em SSR, detalhe, criar e apagar. Texto da pessoa não vira HTML |
| Cron | Uma rotina ligada a uma regra do produto, não a um log "estou vivo". Se o trabalho for pesado ou for um aviso, o cron só enfileira |
| Integração | Pelo menos um destes entra no produto: job na fila, e-mail ou push, métrica num painel. O que ficar de fora continua valendo nos repositórios de estudo |
| Deploy | `docker compose up --build` atrás do Nginx, Postgres sem porta no host |
| Operação | README com variáveis, migration, seed, URL e o aviso do volume |
| Histórico | Commits pequenos a partir do meio do projeto, mensagens que dizem o comportamento |
| Uso | Outra pessoa realizou o fluxo principal e você corrigiu três problemas que ela encontrou |

## Tarefas

### Tarefa 1 — Escopo numa página

**Faça:** `docs/escopo.md` com o problema, quem usa, o fluxo principal em até oito passos, e a seção "não entra nesta versão". Se a lista de "não entra" estiver vazia, o escopo está grande demais: corte até sobrar três exclusões conscientes.

**Pronto quando:** você consegue ler o arquivo em voz alta em menos de três minutos.

### Tarefa 2 — Modelo

**Faça:** desenho das tabelas, colunas, chaves e a regra que mora no banco. Revise com o que você aprendeu de SQL antes de criar entidade.

**Pronto quando:** o desenho está no repositório e uma pessoa que só lê SQL entende.

### Tarefa 3 — Repositório e ambiente

**Faça:** repositório no GitHub, `.gitignore`, `.env.example`, `.nvmrc`, Compose do banco para desenvolver. O primeiro commit é esse esqueleto, não o app inteiro.

**Pronto quando:** clonar e subir o banco funciona, e o app ainda pode nem existir.

### Tarefa 4 — Esquema versionado

**Faça:** entidades, migrations, seed. `synchronize: false` desde o primeiro dia deste projeto. Você já sabe o custo de ligar.

**Pronto quando:** um database novo fica pronto com os scripts npm, duas vezes sem duplicar seed.

### Tarefa 5 — API do fluxo principal

**Faça:** o fluxo da tarefa 1, de ponta a ponta, no Nest, ainda testável com `curl`. Validação, 404, dono ou papel. Teste automático do caso que não pode vazar dado entre contas ou papéis.

**Pronto quando:** o `curl` do README executa o fluxo feliz e um fluxo recusado.

### Tarefa 6 — Conta

**Faça:** o núcleo da sessão 22, no tamanho do produto. Sem segundo fator, sem login social, a menos que o escopo tenha pedido. Cookie de sessão para o navegador. JWT só se houver um cliente que não é o site; se não houver, escreva que ficou de fora e por quê. Não implemente os dois "porque a rubrica antiga da Estante tinha".

**Pronto quando:** a rubrica de auth está satisfeita e o que você não fez está no "não entra".

### Tarefa 7 — Interface

**Faça:** Next.js com TypeScript e Tailwind. A listagem principal chega pronta no HTML. Estados de vazio, erro e validação. Visual legível no celular. Pode ser simples. Precisa ser o seu, não um tema que você não sabe alterar.

**Pronto quando:** o fluxo da tarefa 1 acontece no navegador, com duas contas se o produto tiver conta.

### Tarefa 8 — Rotina agendada

**Faça:** um cron com efeito no domínio (encerrar o que passou do prazo, marcar o que ficou parado, resumir uma contagem que a tela mostra). Se esse efeito for pesado ou disparar um aviso, o cron só enfileira, como na sessão 21. Teste a regra sem depender do relógio. A expressão cron está traduzida no README.

**Pronto quando:** você observa um efeito real no banco, no horário curto de teste, e depois coloca o horário de verdade.

### Tarefa 9 — Fechar a segurança que o produto usa

**Faça:** percorra a tabela de riscos da sessão 22 e marque só as linhas que este produto tem. CORS, cabeçalhos, limite no login, whitelist, CSRF se houver cookie, segredo fora do Git. Não marque linha de feature que você não ligou.

**Pronto quando:** a tabela revisada está em `docs/seguranca.md`.

### Tarefa 10 — Stack única

**Faça:** o compose da sessão 25, ajustado aos nomes deste produto. Só o Nginx publica porta. Migration é comando separado. Backup com `pg_dump` documentado.

**Pronto quando:** `docker compose down` sem `-v` e `up` de novo devolvem você à mesma conta e aos mesmos dados.

### Tarefa 11 — Arquitetura em uma página

**Faça:** `docs/arquitetura.md` com o desenho (navegador, Nginx, Next, Nest, Postgres), onde está cada regra importante, e o que acontece se o banco cair. Uma página. Se passou de duas, está detalhado demais: corte.

**Pronto quando:** o arquivo responde "onde eu mexo se a regra de prazo mudar?" com um caminho de arquivo.

### Tarefa 12 — Outra pessoa usa

**Faça:** alguém que não programou isso tenta o fluxo principal, na sua frente ou por mensagem, a partir do README. Você não ajuda nos primeiros dez minutos, a menos que o README esteja errado: nesse caso, o erro é do README e entra na lista. Anote três problemas. Corrija os três. Commit por correção.

**Pronto quando:** os três problemas e os commits estão citados no README ou numa issue fechada.

### Tarefa 13 — Retrospectiva

**Faça:** `docs/retrospectiva.md`:

- o que você faria diferente se começasse o projeto final hoje;
- qual sessão deste plano você abriria de novo, e por quê;
- três assuntos para os próximos seis meses, escolhidos porque o projeto doeu ali, não porque um ranking de tecnologias mandou.

Sugestões possíveis, só se tiverem doído: teste de interface, fila em vez de cron no processo, permissão mais fina, acessibilidade, HTTPS num servidor. Escolha as suas.

**Pronto quando:** o arquivo existe e a [linha do projeto final](../../PROGRESSO.md) pode ser marcada.

## Checklist

- [ ] 1. Escopo com três exclusões
- [ ] 2. Modelo com regra no banco
- [ ] 3. Repositório sobe o banco no primeiro commit útil
- [ ] 4. Migration e seed
- [ ] 5. API com teste do caso recusado
- [ ] 6. Conta no tamanho do produto
- [ ] 7. Fluxo no navegador, com SSR
- [ ] 8. Cron com efeito real
- [ ] 9. `docs/seguranca.md`
- [ ] 10. Compose atrás do Nginx
- [ ] 11. Arquitetura de uma página
- [ ] 12. Três correções vindas de outra pessoa
- [ ] 13. Retrospectiva

Quando o checklist fechar, marque a sessão 27 em [Progresso](../../PROGRESSO.md). O plano acabou. O repositório do projeto final é o que você mostra quando alguém perguntar o que você estudou.
