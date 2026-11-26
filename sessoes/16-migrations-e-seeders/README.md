# Sessão 16 — Migrations e seeders

**Sessão 16 de 21 · cerca de 2 semanas**

Enquanto `synchronize` está ligado, o TypeORM tenta deixar o banco parecido com as classes. Isso serve para experimentar e machuca quando já existe dado e mais de um ambiente. Migration é um arquivo na ordem do tempo, com um jeito de aplicar e um jeito de desfazer. Seeder coloca os dados iniciais, como as categorias, sem depender de você clicar na tela.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [15 · TypeORM](../15-typeorm/README.md)
> **Próxima →** [17 · Tarefas agendadas](../17-cron-jobs/README.md)

## O que você vai saber fazer

- Desligar `synchronize` e criar o esquema por migration.
- Escrever `up` e `down`.
- Evoluir uma tabela sem editar a migration antiga.
- Rodar um seed idempotente: duas vezes seguidas não duplicam linha.

## Documentação

- [Migrations do TypeORM](https://typeorm.io/docs/advanced-topics/migrations/)
- [Executar e reverter](https://typeorm.io/docs/migrations/executing.md)
- [TypeORM no Nest](https://docs.nestjs.com/data/typeorm), o aviso de produção e a configuração da conexão
- [CLI do TypeORM](https://typeorm.io/docs/advanced-topics/migrations/) — a mesma página descreve `migration:create`, `migration:run` e `migration:revert`. Confira os comandos na versão que você instalou (`npx typeorm --help` ou o script que a documentação indicar)

O TypeORM não traz um comando oficial único de seed. O seed desta sessão é um script seu, usando o `DataSource`, separado das migrations. Migration altera esquema. Seed altera dado. Misturar os dois faz o seed rodar em produção quando você só queria criar tabela.

## Tarefas

### Tarefa 1 — O estrago que o synchronize evitaria mal

**Leia:** o início de [migrations](https://typeorm.io/docs/advanced-topics/migrations/).

**Faça:** no README, o cenário do rename: a coluna `titulo` vira `nome`, já com linhas dentro. Descreva o que você precisa que aconteça com os dados. Não rode esse rename ainda.

**Pronto quando:** o texto diz que a migration carrega o `ALTER TABLE`, e que apagar a tabela não é solução.

### Tarefa 2 — Data source para a CLI

**Faça:** um arquivo `data-source.ts` que a CLI enxerga, com as mesmas credenciais do app (lendo `.env`) e o caminho das migrations. O app Nest e a CLI não podem divergir de usuário e database. Documente o comando.

**Pronto quando:** a CLI conecta. Um erro de credencial é legível.

### Tarefa 3 — Banco limpo

**Faça:** crie um database vazio para esta fase, ou derrube o volume de estudo se não houver dado que você queira. Com `synchronize` ainda `true`, suba o app uma vez para lembrar como a tabela nascia sozinha. Depois desligue `synchronize` em todo lugar (app e data source). Apague as tabelas na mão, no `psql`, neste database de estudo.

**Pronto quando:** subir o app não cria tabela nenhuma. `\dt` não lista `itens`.

### Tarefa 4 — Primeira migration

**Faça:** gere o arquivo com a CLI (`migration:create` ou `migration:generate`, o que você tiver lido). Confira o SQL. Ele cria `itens` e `notas` com as chaves, os `CHECK` e o cascade. `down` derruba na ordem certa (notas antes de itens, se a chave exigir).

Não edite entidade dentro do arquivo de migration. SQL (ou a API do `QueryRunner`) fica estável mesmo se a classe mudar no ano que vem.

**Pronto quando:** `migration:run` cria as tabelas e `migration:revert` remove. Você rodou os dois.

### Tarefa 5 — O app em cima da migration

**Faça:** suba o Nest com `synchronize: false`. Crie um item pela API. A linha aparece no `psql`.

**Pronto quando:** o README diz a ordem: compose, migration, app. Sem o passo do meio, o app falha. Provoque essa falha uma vez e anote a mensagem.

### Tarefa 6 — Segunda migration

**Faça:** acrescente uma coluna de verdade, útil para a Estante, por exemplo `atualizado_em` ou `favorito boolean not null default false`. Altere a entidade. Crie **outra** migration. Não abra a primeira para emendar.

**Pronto quando:** um banco que já rodou a primeira migration consegue rodar só a segunda, e os itens antigos continuam lá, com o valor default.

### Tarefa 7 — O down da segunda

**Faça:** `migration:revert` tira a coluna nova e mantém as linhas. Suba de novo com `migration:run`.

**Pronto quando:** você conferiu no `\d` antes e depois do revert.

### Tarefa 8 — Migration já aplicada não se edita

**Faça:** escreva no README a regra: se a migration já rodou em algum lugar, a correção é uma migration nova. Conte um caso imaginário em que editar a antiga quebraria quem já aplicou.

**Pronto quando:** a regra está em destaque.

### Tarefa 9 — Seed de categorias

**Faça:** se a sua Estante ainda não tem categoria, crie a entidade e a migration da tabela `categorias` (nome único). O seed insere três categorias do seu tema.

O script:

- conecta com o `DataSource`;
- insere pelo nome, sem duplicar (`INSERT ... ON CONFLICT` ou um `find` antes, usando o que você já sabe de SQL);
- desconecta no final, inclusive se der erro.

**Pronto quando:** rodar o seed duas vezes deixa três linhas, não seis.

### Tarefa 10 — Scripts npm

**Faça:**

- `npm run migration:run`
- `npm run migration:revert`
- `npm run seed`

Cada um chama a CLI ou o script com o data source certo. O README lista a ordem para uma máquina nova: `up` do compose, migration, seed, start.

**Pronto quando:** você segue essa ordem num database recém-criado e a API lista as três categorias.

### Tarefa 11 — O que não entra no seed

**Faça:** o seed não cria usuário administrador com senha conhecida. Isso fica para a sessão 18, e mesmo lá a senha não mora no Git. O seed desta sessão é dado de catálogo, não credencial.

**Pronto quando:** o README diz isso numa frase, e o script não contém senha.

## Checklist

- [ ] 1. Cenário do rename escrito
- [ ] 2. CLI conecta no mesmo database do app
- [ ] 3. `synchronize` desligado e tabela não nasce sozinha
- [ ] 4. `up` e `down` da primeira migration
- [ ] 5. Ordem compose → migration → app
- [ ] 6. Segunda migration sem editar a primeira
- [ ] 7. Revert confere no `\d`
- [ ] 8. Regra da migration aplicada
- [ ] 9. Seed idempotente
- [ ] 10. Scripts e máquina nova
- [ ] 11. Seed sem senha

## Próximo passo

Siga para [17 · Tarefas agendadas](../17-cron-jobs/README.md).
