# Sessão 9 — Banco de dados com SQLite

**Sessão 9 de 27 · cerca de 3 semanas**

O arquivo JSON guarda tudo num bloco só. Um banco relacional guarda linhas, exige colunas e permite perguntar "só os que estão em andamento, em ordem". Você aprende SQL no SQLite, que é um arquivo único, e liga a CLI da Estante nele.

As sessões 07 e 08 trataram do tempo: esperar uma operação e não travar a thread. Aqui o assunto volta a ser o dado.

O PostgreSQL vem na próxima sessão. O SQL que você escrever aqui volta lá com pouca diferença.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [08 · Concorrência e paralelismo](../08-concorrencia-e-paralelismo/README.md)
> **Próxima →** [10 · Docker e PostgreSQL](../10-docker-e-postgresql/README.md)

## O que você vai saber fazer

- Criar tabela, inserir, consultar, atualizar e apagar.
- Usar chave primária, `NOT NULL`, `UNIQUE` e chave estrangeira.
- Juntar duas tabelas com `JOIN`.
- Fazer a CLI falar com o banco pelo módulo `node:sqlite`.

## Documentação

- [SQLite em cinco minutos](https://www.sqlite.org/quickstart.html)
- [Linguagem SQL do SQLite](https://www.sqlite.org/lang.html)
- [CREATE TABLE](https://www.sqlite.org/lang_createtable.html)
- [Chaves estrangeiras](https://www.sqlite.org/foreignkeys.html)
- [SELECT](https://www.sqlite.org/lang_select.html)
- [`node:sqlite`](https://nodejs.org/api/sqlite.html) — leia esta página na versão do Node que o `node -v` mostrar. Se a documentação da sua versão marcar o módulo como experimental, use o comando de ativação que ela indicar e anote isso no README

Instalação do cliente no CachyOS:

```bash
sudo pacman -S sqlite
sqlite3 -version
```

Referência de pacote, se precisar: [pacman](https://wiki.archlinux.org/title/Pacman).

## Tarefas

Trabalhe numa branch `sqlite` de `estante-cli`, ou num repositório `estante-sqlite` copiando a CLI. Escolha um e escreva no README. Os testes de regra pura continuam valendo; os testes de arquivo JSON passam a ter um equivalente de banco.

### Tarefa 1 — O que muda em relação ao JSON

**Faça:** no README, quatro linhas comparando: onde mora o dado, o que acontece se duas pessoas editarem ao mesmo tempo (ainda não é o seu caso, mas descreva), como você busca um item, como você impede um título vazio.

**Pronto quando:** a comparação existe antes de você abrir o `sqlite3`.

### Tarefa 2 — Primeira tabela, no terminal

**Leia:** [quickstart](https://www.sqlite.org/quickstart.html).

**Faça:**

```bash
mkdir -p dados
sqlite3 dados/estante.db
```

Dentro do shell:

```sql
CREATE TABLE itens (
  id INTEGER PRIMARY KEY,
  titulo TEXT NOT NULL,
  ano INTEGER,
  estado TEXT NOT NULL DEFAULT 'quero',
  criado_em TEXT NOT NULL
);
INSERT INTO itens (titulo, ano, estado, criado_em)
VALUES ('O nome da rosa', 1980, 'quero', '2026-01-01T00:00:00Z');
SELECT * FROM itens;
.quit
```

**Pronto quando:** sair e entrar de novo ainda mostra a linha. O banco é um arquivo.

### Tarefa 3 — Consultas

**Leia:** [SELECT](https://www.sqlite.org/lang_select.html).

**Faça:** insira pelo menos seis linhas na mão. Escreva e salve num arquivo `sql/consultas.sql`:

- todos os títulos em ordem alfabética;
- só um estado;
- busca com `LIKE`;
- contagem por estado (`GROUP BY`);
- o item mais recente (`ORDER BY criado_em DESC LIMIT 1`).

**Pronto quando:** você roda o arquivo com `sqlite3 dados/estante.db < sql/consultas.sql` e confere os resultados.

### Tarefa 4 — Update, delete e uma regra

**Faça:** avance o estado de um id com `UPDATE`. Apague outro com `DELETE`. Tente inserir título nulo e registre, no README, a mensagem do SQLite.

**Pronto quando:** a tentativa inválida foi recusada pelo banco, não só pelo seu código.

### Tarefa 5 — Restringir o estado

**Leia:** a parte de `CHECK` em [CREATE TABLE](https://www.sqlite.org/lang_createtable.html).

**Faça:** recrie a tabela (o banco ainda é de estudo) com `CHECK (estado IN ('quero', 'fazendo', 'feito'))`. Tente gravar `estado = 'talvez'`.

**Pronto quando:** o banco recusa o valor e o SQL dessa criação está em `sql/schema.sql`.

### Tarefa 6 — Segunda tabela

**Leia:** [chaves estrangeiras](https://www.sqlite.org/foreignkeys.html). No SQLite elas precisam estar ligadas (`PRAGMA foreign_keys = ON`) em cada conexão.

**Faça:** a tabela `notas` com `id`, `item_id`, `texto` e `criado_em`. `item_id` referencia `itens(id)`. Defina o que acontece ao apagar o item (`ON DELETE CASCADE` é uma escolha boa aqui: a nota não faz sentido sem o item). Teste apagando um item que tem nota.

**Pronto quando:** `schema.sql` cria as duas tabelas, e você anotou que esqueceu o `PRAGMA` se a primeira tentativa deixou passar um `item_id` inventado.

### Tarefa 7 — JOIN

**Faça:** uma consulta que lista cada nota junto do título do item. Outra que lista itens e a quantidade de notas, inclusive item com zero (`LEFT JOIN`).

**Pronto quando:** as duas consultas estão em `sql/consultas.sql` e o resultado confere com o que você inseriu.

### Tarefa 8 — Ligar o Node

**Leia:** [`node:sqlite`](https://nodejs.org/api/sqlite.html) na sua versão.

**Faça:** um módulo `src/banco.js` que abre `dados/estante.db`, liga as chaves estrangeiras e exporta funções `listar`, `inserir`, `avancar`, `remover`. Os comandos da CLI passam a usar esse módulo. O JSON fica para trás: pode manter o código antigo numa pasta `arquivo-json/` ou apagar nesta branch, desde que o README diga.

**Pronto quando:** `node estante.js listar` lê do banco. Você confirma abrindo o mesmo arquivo no `sqlite3`.

### Tarefa 9 — Notas pela CLI

**Faça:**

```bash
node estante.js anotar 1 "capítulo 1 estranho"
node estante.js notas 1
```

**Pronto quando:** a nota aparece no `JOIN` da tarefa 7.

### Tarefa 10 — Testes com banco temporário

**Faça:** cada teste cria um arquivo `.db` vazio num diretório temporário, aplica `schema.sql` e apaga no final. Não use `dados/estante.db` nos testes.

**Pronto quando:** `npm test` passa duas vezes seguidas sem vazar linha de um teste para o outro.

### Tarefa 11 — Transação pequena

**Leia:** a página de transações do SQLite, linkada a partir de [lang.html](https://www.sqlite.org/lang.html) (`BEGIN`).

**Faça:** `remover` apaga notas e item na mesma transação, se você não usou `ON DELETE CASCADE`. Se usou cascade, faça outra operação em duas etapas (inserir item e a primeira nota) e desfaça as duas se a nota vier vazia.

**Pronto quando:** o caso de falha não deixa meia escrita. Um teste mostra isso.

### Tarefa 12 — Backup

**Faça:** copie o arquivo `.db` para `dados/backup/`. Documente o comando. Abra a cópia e conte as linhas.

O `.db` pessoal fica no `.gitignore`. O `schema.sql` entra no Git.

**Pronto quando:** apagar o arquivo original e repor a cópia devolve a coleção.

### Tarefa 13 — O que o SQLite não é, neste plano

**Faça:** escreva cinco linhas no README sobre o limite que você já consegue imaginar: um arquivo local, um processo de cada vez escrevendo com conforto, sem usuários e senhas de banco. A sessão 10 coloca um servidor de banco dentro de um container.

**Pronto quando:** esse parágrafo existe e cita o que você de fato usou, não uma lista genérica decorada.

## Checklist

- [ ] 1. Comparação com JSON
- [ ] 2. Tabela criada e relida
- [ ] 3. Cinco consultas num arquivo `.sql`
- [ ] 4. Update, delete e título nulo recusado
- [ ] 5. `CHECK` do estado
- [ ] 6. Notas com chave estrangeira
- [ ] 7. `JOIN` e `LEFT JOIN`
- [ ] 8. CLI lendo o SQLite
- [ ] 9. Comando de anotar
- [ ] 10. Testes em banco temporário
- [ ] 11. Transação com teste de falha
- [ ] 12. Backup do arquivo
- [ ] 13. Limites escritos no README

## Próximo passo

Siga para [10 · Docker e PostgreSQL](../10-docker-e-postgresql/README.md).
