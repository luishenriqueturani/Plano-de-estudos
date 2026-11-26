# Sessão 08 — Docker e PostgreSQL

**Sessão 8 de 21 · cerca de 3 semanas**

O SQLite era um arquivo. O PostgreSQL é um servidor: ele fica ligado, escuta uma porta, tem usuário e senha. O Docker sobe esse servidor num container, com um volume para os dados não sumirem quando o container for recriado.

A CLI da Estante passa a falar com o Postgres. Guarde o SQLite no histórico do Git. Não precisa apagar o aprendizado.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [07 · SQLite](../07-sqlite/README.md)
> **Próxima →** [09 · Cliente e servidor](../09-cliente-servidor/README.md)

## O que você vai saber fazer

- Instalar o Docker no CachyOS e rodar um container.
- Subir o PostgreSQL com Compose, usuário próprio e volume.
- Recriar o esquema da Estante em SQL do Postgres.
- Entrar no banco com `psql`, consultar e fazer um backup.

## Documentação

- [Docker no Arch](https://wiki.archlinux.org/title/Docker) — instalação, serviço e rodar sem `sudo` em todo comando
- [Começar com o Docker](https://docs.docker.com/get-started/)
- [Compose](https://docs.docker.com/compose/gettingstarted/)
- [Imagem oficial do PostgreSQL](https://hub.docker.com/_/postgres)
- [Tutorial do PostgreSQL](https://www.postgresql.org/docs/current/tutorial.html)
- [Criar tabelas](https://www.postgresql.org/docs/current/ddl.html)
- [Inserir e alterar](https://www.postgresql.org/docs/current/dml.html)
- [Consultas](https://www.postgresql.org/docs/current/queries.html)
- [Papéis e privilégios](https://www.postgresql.org/docs/current/user-manag.html)
- [Backup com pg_dump](https://www.postgresql.org/docs/current/backup-dump.html)

Não instale o pacote `postgresql` do sistema nesta sessão. O servidor mora no container. O cliente `psql` pode ser o do próprio container, assim você não precisa de outro pacote.

## Tarefas

### Tarefa 1 — Container, imagem e volume

**Leia:** a visão geral em [Get Started](https://docs.docker.com/get-started/).

**Faça:** no README de um repositório novo `estante-dados` (ou na pasta desta fase), defina com as suas palavras: imagem, container e volume. Uma frase cada, com a Estante como exemplo ("a imagem é o Postgres pronto", e assim por diante).

**Pronto quando:** as três frases existem antes do `docker run`.

### Tarefa 2 — Instalar o Docker

**Leia:** [Docker — ArchWiki](https://wiki.archlinux.org/title/Docker), instalação e a parte de executar como usuário normal.

**Faça:** instale o engine e o plugin Compose v2 pelos pacotes que a wiki indicar. Habilite o serviço. Se a wiki pedir o grupo `docker` e um novo login, faça isso e só então teste.

```bash
docker version
docker compose version
docker run --rm archlinux bash -c "echo olá do container"
```

O último comando está na própria wiki, como teste. A imagem `archlinux` é grande na primeira vez: isso é o download, não um erro.

**Pronto quando:** os três comandos funcionam sem `sudo`, depois do que a wiki exigir para o seu usuário.

### Tarefa 3 — Olhar o que ficou rodando

**Faça:**

```bash
docker ps
docker ps -a
docker images
```

Suba um container que permanece um pouco (a documentação de `docker run` explica `-d` e um processo que não termina na hora, como `docker run -d --name teste nginx` se você já tiver curiosidade, ou simplesmente observe o `archlinux` que já saiu). Pare e remova o que você criou com nome.

**Pronto quando:** você explica a diferença entre `ps` e `ps -a` nas suas notas, com um container que você mesmo parou.

### Tarefa 4 — Postgres pela primeira vez

**Leia:** as variáveis `POSTGRES_USER`, `POSTGRES_PASSWORD` e `POSTGRES_DB` na [imagem oficial](https://hub.docker.com/_/postgres).

**Faça:** escolha uma senha só para a sua máquina. Não cole essa senha no Git.

```bash
docker run -d --name estante-pg \
  -e POSTGRES_USER=estante \
  -e POSTGRES_PASSWORD=coloque-a-sua \
  -e POSTGRES_DB=estante \
  -p 5432:5432 \
  postgres:17
```

A tag `17` pode ser a versão estável atual que o Docker Hub mostrar quando você ler a página. Anote a tag que usou.

**Pronto quando:** `docker ps` mostra o container e `docker logs estante-pg` diz que o banco está pronto para conexões.

### Tarefa 5 — psql dentro do container

**Leia:** o começo do [tutorial](https://www.postgresql.org/docs/current/tutorial.html).

**Faça:**

```bash
docker exec -it estante-pg psql -U estante -d estante
```

Rode `\dt`, `SELECT version();` e `\q`.

**Pronto quando:** você entrou, viu a versão e saiu.

### Tarefa 6 — O esquema de novo

**Leia:** [DDL](https://www.postgresql.org/docs/current/ddl.html).

**Faça:** traduza `schema.sql` do SQLite para Postgres. Diferenças que você deve encontrar e anotar:

- chave primária com `GENERATED ... AS IDENTITY` ou `SERIAL` (prefira `IDENTITY`, que a documentação apresenta como a forma atual);
- `TEXT` continua existindo;
- booleanos e datas: `criado_em TIMESTAMPTZ NOT NULL DEFAULT now()`;
- o `CHECK` do estado permanece.

Salve em `sql/schema.sql` e aplique com `\i` ou redirecionando o arquivo pelo `docker exec`.

**Pronto quando:** `\dt` lista `itens` e `notas`, e um `INSERT` com estado inválido falha.

### Tarefa 7 — Usuário da aplicação

**Leia:** [papéis](https://www.postgresql.org/docs/current/user-manag.html).

**Faça:** crie um papel `estante_app` com senha, permissão de conectar no banco e de usar as tabelas. A CLI não entra mais como o usuário dono do banco. Anote os `GRANT` no `schema.sql` ou num `sql/permissoes.sql`.

**Pronto quando:** `psql` com `estante_app` consegue `SELECT` e `INSERT`, e você testou uma operação que ele não deve poder (por exemplo, apagar o banco) e ela falhou.

### Tarefa 8 — Volume, o dado sobrevive

**Faça:** pare e remova o container da tarefa 4. Suba outro com o mesmo comando. Entre no `psql`. As tabelas sumiram: sem volume, o dado estava na camada gravável do container.

Agora suba com um volume nomeado (`-v estante-dados:/var/lib/postgresql/data`, conferindo o caminho na documentação da tag que você escolheu). Crie as tabelas de novo, insira uma linha, remova o container e suba outro apontando para o mesmo volume.

**Pronto quando:** a linha continua lá depois de recriar o container. As notas descrevem o que o volume segurou.

### Tarefa 9 — Compose

**Leia:** [Compose getting started](https://docs.docker.com/compose/gettingstarted/).

**Faça:** `compose.yaml` com o serviço `postgres`, a imagem, o volume, a porta e `env_file: .env`. O `.env` fica no `.gitignore`. Commite um `.env.example` com as chaves e valores falsos óbvios, do tipo `troque-esta-senha`.

```bash
docker compose up -d
docker compose ps
docker compose logs postgres
docker compose down
```

`down` sem `-v` não apaga o volume. Confira isso de propósito: `down`, `up`, a linha ainda existe. Depois leia o que `down -v` faria e não rode no banco em que você já se importa, só num teste.

**Pronto quando:** o ciclo `up` / `down` / `up` preserva os dados, e o README diz o comando que apagaria o volume.

### Tarefa 10 — A CLI conecta

**Leia:** o driver que você for usar. Opções honestas:

- o cliente `pg` em JavaScript, documentação em [node-postgres](https://node-postgres.com/);
- ou ficar no SQL via `docker exec` só até a sessão 10.

Nesta tarefa, use `pg`. Instale com npm, guarde usuário e senha em variável de ambiente (`process.env`), nunca no código.

**Faça:** `listar` e `adicionar` contra o Postgres. O restante dos comandos pode migrar na tarefa 11.

**Pronto quando:** uma linha inserida pela CLI aparece no `psql`.

### Tarefa 11 — O resto dos comandos

**Faça:** `avancar`, `remover`, `anotar`, `notas` e `buscar` no Postgres. Reaproveite as funções puras da sessão 06. Troque só a borda que fala com o banco.

**Pronto quando:** a sequência da sessão 06 funciona de ponta a ponta e os testes usam um banco de teste (outro database no mesmo servidor, criado e dropado pelo teste, ou um compose separado). Documente qual você escolheu.

### Tarefa 12 — Uma diferença que mordeu

**Faça:** escreva no README uma diferença real que você encontrou entre SQLite e Postgres (placeholder `$1` em vez de `?`, `RETURNING`, maiúsculas em nomes não citados, `TIMESTAMPTZ`, o `PRAGMA` que não existe mais). Se nenhuma te mordeu, provoque o placeholder errado de propósito e anote o erro.

**Pronto quando:** a nota cita a mensagem de erro ou o resultado diferente.

### Tarefa 13 — Backup

**Leia:** [pg_dump](https://www.postgresql.org/docs/current/backup-dump.html).

**Faça:**

```bash
docker exec estante-pg pg_dump -U estante -d estante > backup.sql
```

Ajuste o nome do container se o Compose tiver prefixado. Restaure num banco vazio e conte as linhas.

O `backup.sql` com os seus dados reais não vai para o Git. O comando vai para o README.

**Pronto quando:** a restauração devolve a mesma contagem.

### Tarefa 14 — README de operação

**Faça:** a seção "como subir o banco" com a ordem exata: copiar `.env.example`, `docker compose up -d`, aplicar o schema, rodar a CLI. Inclua como parar e como ver os logs.

**Pronto quando:** você segue o próprio README numa pasta clonada em `/tmp` e chega num `listar` vazio sem consultar as notas antigas.

## Checklist

- [ ] 1. Imagem, container e volume definidos
- [ ] 2. Docker instalado pela wiki
- [ ] 3. `ps` e `ps -a` diferenciados
- [ ] 4. Postgres no ar
- [ ] 5. `psql` usado
- [ ] 6. Esquema traduzido
- [ ] 7. Papel `estante_app` com menos poder
- [ ] 8. Volume testado com recriação
- [ ] 9. Compose e `.env.example`
- [ ] 10. CLI insere e o `psql` vê
- [ ] 11. Demais comandos migrados
- [ ] 12. Uma diferença real anotada
- [ ] 13. `pg_dump` restaurado
- [ ] 14. README seguido do zero

## Próximo passo

Siga para [09 · Cliente e servidor](../09-cliente-servidor/README.md). O Compose continua de pé: a API vai usar este banco.
