# Sessão 19 — TypeORM

**Sessão 19 de 28 · cerca de 3 semanas**

O service deixa o array e passa a usar o PostgreSQL que já está no Docker. O TypeORM representa tabelas como classes. Você continua sabendo SQL: a tarefa final desta sessão é ler a query que o ORM gerou e reconhecer o `SELECT` da sessão 10.

`synchronize: true` pode ligar no começo, só na sua máquina, para a tabela nascer sozinha. A sessão 20 desliga isso e troca por migrations. Não pule essa combinação: ligar agora sem desligar depois é como a documentação avisa, um jeito de perder dado.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [18 · NestJS](../18-nestjs/README.md)
> **Próxima →** [20 · Migrations e seeders](../20-migrations-e-seeders/README.md)

## O que você vai saber fazer

- Conectar o Nest ao Postgres com `@nestjs/typeorm`.
- Mapear `Item` e `Nota` como entidades, com relação um-para-muitos.
- Fazer o CRUD pelo repositório.
- Devolver um objeto de saída, não a entidade crua, quando os dois divergirem.
- Ler o SQL logado e comparar com o que você escreveria.

## Documentação

- [TypeORM no Nest](https://docs.nestjs.com/data/typeorm)
- [Começar com o TypeORM](https://typeorm.io/docs/getting-started.md)
- [Entidades](https://typeorm.io/docs/entity/entities.md)
- [Relações um-para-muitos](https://typeorm.io/docs/relations/many-to-one-one-to-many-relations.md)
- [Relações muitos-para-muitos](https://typeorm.io/docs/relations/many-to-many-relations.md)
- [Repositório e find](https://typeorm.io/docs/working-with-entity-manager/repository-api.md) — se o caminho mudar, busque "Repository API" em [typeorm.io](https://typeorm.io/)
- [Query builder](https://typeorm.io/docs/query-builder/select-query-builder.md)

O aviso sobre `synchronize` está na página do Nest: não usar em produção. Nesta sessão, só no desenvolvimento, e por pouco tempo.

## Tarefas

Continue em `estante-nest`. O Compose sobe o Postgres. Use o papel de aplicação da sessão 10, não o superusuário.

### Tarefa 1 — O que o ORM faz e o que ele não faz

**Leia:** a introdução em [getting started](https://typeorm.io/docs/getting-started.md).

**Faça:** no README, duas colunas: "o TypeORM faz" (montar SQL, hidratar objeto, acompanhar entidade) e "continua sendo meu" (regra de avançar estado, o que a API devolve, a senha do banco). 

**Pronto quando:** a regra de estado não foi parar num lugar mágico. Você já sabe em qual service ela vai ficar.

### Tarefa 2 — Conexão

**Leia:** [TypeORM no Nest](https://docs.nestjs.com/data/typeorm). Instale o driver do Postgres (`pg`), não o de MySQL que alguns exemplos usam.

**Faça:** `TypeOrmModule.forRoot` (ou `forRootAsync` lendo `ConfigService`) com host, porta, usuário, senha, database, `synchronize: true` e `logging: true`. Entidades registradas. Suba o app e ache no log a conexão bem-sucedida.

**Pronto quando:** uma senha errada no `.env` falha na subida, e a certa conecta.

### Tarefa 3 — Entidade Item

**Leia:** [entidades](https://typeorm.io/docs/entity/entities.md).

**Faça:** a classe `Item` com colunas equivalentes ao `schema.sql`. O estado é uma coluna string com o mesmo conjunto de antes. Confira no `psql` se a tabela nasceu. Se já existir uma tabela incompatível de sessões anteriores, use um database novo (`estante_nest`) para não brigar com o esquema antigo. Documente o nome.

**Pronto quando:** `\d itens` mostra as colunas que a entidade descreve.

### Tarefa 4 — Repositório no service

**Faça:** injete `Repository<Item>`. `listar`, `buscar`, `criar`, `atualizar` e `remover` usam o repositório. O controller muda pouco: ele já falava com o service.

**Pronto quando:** reiniciar o Nest preserva os itens, e os testes de service foram ajustados. Teste pode usar o SQLite em memória que o TypeORM suporta, ou um Postgres de teste. Escolha um, documente, e não aponte teste para o database em que você guarda dados seus.

### Tarefa 5 — Not found e conflito

**Faça:** id ausente continua 404. Se você decidir que título é único, o banco tem a restrição e o service traduz o erro do Postgres para 409. Se título não for único, escreva isso e não invente conflito.

**Pronto quando:** o comportamento documentado tem um teste.

### Tarefa 6 — Relação com Nota

**Leia:** [one-to-many](https://typeorm.io/docs/relations/many-to-one-one-to-many-relations.md).

**Faça:** `Nota` com muitos-para-um em `Item`, e um-para-muitos no `Item`. Apagar o item apaga as notas (`onDelete: 'CASCADE'` ou o equivalente que a documentação mostrar). Carregar o item com as notas usa `relations`, não uma consulta preguiçosa acidental dentro de um laço.

**Pronto quando:** o log mostra um JOIN ou duas queries conscientes, e você diz qual dos dois aconteceu.

### Tarefa 7 — Busca

**Faça:** `GET /itens?estado=fazendo&q=rosa` filtra no banco, não na lista já carregada. `ILike` ou `QueryBuilder`, o que a documentação do TypeORM apresentar para Postgres.

**Pronto quando:** uma coleção com mais itens do que você quer trazer para a memória ainda filtra certo. Pode inserir trinta linhas num seed manual.

### Tarefa 8 — Saída da API

**Faça:** um tipo ou classe de resposta com os campos públicos. O controller devolve isso. Se a entidade ganhar uma coluna interna no futuro, ela não vaza só porque existe na tabela. Hoje a diferença pode ser pequena (`criadoEm` em camelCase). O hábito é o ponto.

**Pronto quando:** a resposta JSON não é um objeto de entidade com métodos, e um teste olha as chaves.

### Tarefa 9 — Transação

**Leia:** a parte de transações na documentação do TypeORM (busque "transactions" em [typeorm.io](https://typeorm.io/)).

**Faça:** criar item com a primeira nota, se as duas escritas forem uma operação só no seu desenho, roda numa transação. Se a nota falhar validação, não fica item órfão. Se você não tiver esse caso, faça a remoção de item e notas na mesma transação e teste a falha no meio.

**Pronto quando:** o teste de falha mostra o banco como estava antes.

### Tarefa 10 — SQL à vista

**Faça:** copie do log uma query gerada pelo listar com filtro e reescreva, no README, o SQL equivalente que você teria digitado no `psql`. Aponte uma diferença (aspas, alias, limite).

**Pronto quando:** as duas versões estão lado a lado.

### Tarefa 11 — synchronize ainda ligado, e a data de desligar

**Faça:** no README, em destaque: `synchronize` está `true` só até a sessão 20. Escreva o que ele faz ao renomear uma coluna (dica da própria documentação do Nest: não é um caminho seguro quando já há dados).

**Pronto quando:** esse aviso está no topo do README, não no fim.

## Checklist

- [ ] 1. Limites do ORM escritos
- [ ] 2. Conexão com `pg` e senha errada testada
- [ ] 3. Entidade confere com `\d`
- [ ] 4. CRUD no repositório, teste fora do banco pessoal
- [ ] 5. 404 e, se couber, 409
- [ ] 6. Notas em relação, sem consulta em laço
- [ ] 7. Filtro no banco
- [ ] 8. Objeto de saída
- [ ] 9. Transação com teste de falha
- [ ] 10. SQL do log comparado ao seu
- [ ] 11. Aviso do `synchronize` no topo do README

## Próximo passo

Siga para [20 · Migrations e seeders](../20-migrations-e-seeders/README.md).
