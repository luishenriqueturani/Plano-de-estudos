# Sessão 06 — Node.js e lógica de programação

**Sessão 6 de 28 · cerca de 6 semanas**

A Estante vira um programa de linha de comando: incluir, listar, concluir e apagar itens num arquivo JSON. O assunto novo, por baixo dos comandos, é lógica: quebrar um problema, dar nome ao dado e testar o pedaço sem abrir o navegador.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [05 · Ambiente Node.js](../05-ambiente-nodejs/README.md)
> **Próxima →** [07 · Async e await](../07-async-e-await/README.md)

## O que você vai saber fazer

- Dividir um problema em funções com entrada e saída claras.
- Ler e escrever arquivo com o módulo `node:fs`.
- Tratar erro esperado (arquivo ausente, JSON inválido, comando desconhecido).
- Cobrir as funções puras com o test runner que já vem no Node.
- Usar `async/await` para gravar o arquivo. O modelo do event loop fica na sessão seguinte.

## Documentação

- [Módulos nativos com o prefixo `node:`](https://nodejs.org/api/modules.html)
- [Sistema de arquivos](https://nodejs.org/api/fs.html) — comece pelos métodos de promise (`fs/promises`)
- [Linha de comando](https://nodejs.org/api/process.html#processargv)
- [Test runner](https://nodejs.org/api/test.html)
- [Guia de JavaScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide), de novo, agora sem DOM
- [JavaScript assíncrono](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Async_JS/Introducing)
- [npm scripts](https://docs.npmjs.com/cli/v10/using-npm/scripts/) — se a versão da página mudar, abra [docs.npmjs.com](https://docs.npmjs.com/) e busque "scripts"

## O programa que vai existir

```bash
node estante.js adicionar "O nome da rosa" --ano 1980
node estante.js listar
node estante.js listar --estado fazendo
node estante.js avancar 3
node estante.js remover 3
```

Os dados ficam em `dados/estante.json`, que não entra no Git. Entre no Git um `dados/estante.exemplo.json`.

## Tarefas

### Tarefa 1 — O problema no papel

**Faça:** antes de codar, escreva no README:

- quais comandos existem;
- quais argumentos cada um recebe;
- o que é um item (campos e tipos);
- quais erros o usuário pode provocar.

**Pronto quando:** essa especificação cabe numa tela e você ainda não criou `estante.js`.

### Tarefa 2 — Decompor

**Faça:** liste as funções, ainda sem implementação. Separe as que não sabem de arquivo (`criarItem`, `avancarEstado`, `filtrar`) das que sabem (`carregar`, `salvar`). Desenhe quem chama quem.

**Pronto quando:** o desenho está no README e nenhuma função "faz tudo".

### Tarefa 3 — Exercícios curtos de lógica

Num arquivo `exercicios.js`, resolva e rode. Não busque a resposta pronta; o valor está em quebrar o problema.

1. Dado um número, imprima de 1 até ele, trocando múltiplos de 3 por `Fizz`, de 5 por `Buzz` e de ambos por `FizzBuzz`.
2. Diga se uma palavra é palíndromo, ignorando maiúsculas.
3. Conte as vogais de uma frase.
4. Devolva o maior número de uma lista. Lista vazia é um erro seu, com mensagem.
5. Agrupe uma lista de itens por estado e devolva um objeto `{ quero: [...], fazendo: [...], feito: [...] }`.

**Pronto quando:** os cinco rodam com um caso normal e um caso chato (lista vazia, frase sem vogal, palavra com acento se você decidir suportá-lo).

### Tarefa 4 — Testar os exercícios

**Leia:** [Test runner](https://nodejs.org/api/test.html), o bastante para `describe`, `it` e `assert`.

**Faça:** `exercicios.test.js` com dois testes para o agrupamento e um para a lista vazia do maior número. Script npm `"test": "node --test"`.

**Pronto quando:** `npm test` passa, e quebrar a função de propósito faz o teste falhar.

### Tarefa 5 — Módulos

**Leia:** [Modules: CommonJS modules](https://nodejs.org/api/modules.html) e [Packages](https://nodejs.org/api/packages.html), a parte de `"type": "module"`.

**Faça:** o projeto usa ESM (`"type": "module"` no `package.json`). `exercicios.js` exporta as funções. O teste importa. O arquivo que só roda o exercício manual não mistura com o que é exportado.

**Pronto quando:** `npm test` continua passando com `import`/`export`.

### Tarefa 6 — Funções da Estante, ainda sem arquivo

**Faça:** implemente e teste:

- `criarItem({ titulo, ano })` gera `id`, estado inicial `quero` e recusa título vazio;
- `avancarEstado(item)` segue `quero` → `fazendo` → `feito` e não passa de `feito`;
- `removerPorId(lista, id)` devolve a lista nova sem mutar a antiga, ou muta de propósito e o README assume isso. Escolha um estilo e fique nele.

**Pronto quando:** há testes para título vazio, avanço até o fim e id inexistente.

### Tarefa 7 — Ler e escrever JSON

**Leia:** `fsPromises.readFile` e `writeFile` em [fs](https://nodejs.org/api/fs.html).

**Faça:** `carregar(caminho)` e `salvar(caminho, lista)`. Se o arquivo não existir, `carregar` devolve `[]`. Se o JSON for inválido, o erro chega com uma mensagem que cita o arquivo.

Use `async/await`.

**Pronto quando:** um teste usa um arquivo temporário (pasta `os.tmpdir()` ou um arquivo em `tmp/` ignorado pelo Git), grava duas vezes e lê de volta.

### Tarefa 8 — Argumentos

**Leia:** [process.argv](https://nodejs.org/api/process.html#processargv).

**Faça:** uma função `interpretar(argv)` que, a partir do array, devolve um objeto `{ comando, ... }`. Não leia `process.argv` dentro dela: receba o array, para poder testar.

Casos: comando ausente, `listar`, `listar` com `--estado`, `adicionar` sem título.

**Pronto quando:** os casos têm teste e nenhum deles precisa executar o programa inteiro.

### Tarefa 9 — Juntar

**Faça:** `estante.js` lê os argumentos, carrega, chama a função, salva quando houve mudança e imprime o resultado. `listar` não salva.

**Pronto quando:** a sequência da seção "o programa que vai existir" funciona no seu terminal, com um item real.

### Tarefa 10 — Mensagens para humano

**Faça:** comando desconhecido imprime os comandos aceitos e termina com código diferente de zero (`process.exitCode = 1`). Lista vazia imprime uma frase, não `[]`. Erro de validação não despeja stacktrace em cima do usuário; o stacktrace pode ir para o stderr se você quiser, a frase clara é obrigatória.

**Pronto quando:** você provoca os três casos e a saída é legível.

### Tarefa 11 — Datas e ids

**Faça:** `id` novo é o maior id atual mais um, começando em 1. Grave também `criadoEm` com `new Date().toISOString()`. `listar` mostra id, título, ano e estado, alinhados o bastante para ler. Não precisa de biblioteca de tabela.

**Pronto quando:** dois itens seguidos não repetem id, mesmo depois de apagar o do meio.

### Tarefa 12 — Busca

**Faça:** `node estante.js buscar "rosa"` lista títulos que contêm o texto, sem diferenciar maiúsculas.

**Pronto quando:** há teste da função de busca cobrindo maiúsculas e texto ausente.

### Tarefa 13 — npm scripts

**Faça:**

- `"start"` não se aplica bem a uma CLI com argumentos. Use `"estante"` via [npm exec ou um script](https://docs.npmjs.com/) que encaminha argumentos, ou documente `node estante.js` como comando oficial.
- `"test"` roda a suíte.
- `"exemplo"` recria `dados/estante.json` a partir do exemplo, se você quiser esse atalho.

**Pronto quando:** o README tem os comandos copiáveis e você os rodou.

### Tarefa 14 — Organização da pasta

```text
estante-cli/
  estante.js
  src/itens.js
  src/arquivo.js
  src/cli.js
  test/itens.test.js
  test/cli.test.js
  dados/estante.exemplo.json
  .gitignore
  .nvmrc
  package.json
  README.md
```

Pode ajustar nomes. Não pode: um único arquivo com centenas de linhas.

**Pronto quando:** a árvore real está no README e bate com a pasta.

### Tarefa 15 — Repositório remoto

**Faça:** crie `estante-cli` no GitHub, envie a `main`, confira que `dados/estante.json` e `node_modules` ficaram de fora. O exemplo JSON entrou.

**Pronto quando:** clonar em `/tmp`, rodar `nvm use` e `node estante.js listar` chega numa lista vazia ou no exemplo, seguindo o README.

### Tarefa 16 — Opcional: desfazer o último avanço

**Faça:** guarde `estadoAnterior` ou simplesmente decida a regra inversa, com teste. Se a regra ficar ambígua, escreva no README que esta versão não desfaz, e cancele a tarefa sem culpa.

**Pronto quando:** ou o comando `voltar` tem teste, ou o README diz que ficou de fora.

## Checklist

- [ ] 1. Especificação antes do código
- [ ] 2. Funções desenhadas
- [ ] 3. Cinco exercícios de lógica
- [ ] 4. Testes dos exercícios
- [ ] 5. ESM ligado
- [ ] 6. Regras da Estante testadas
- [ ] 7. Arquivo JSON com erro tratado
- [ ] 8. Argumentos testados sem rodar o processo
- [ ] 9. Comandos reais no terminal
- [ ] 10. Erros legíveis e código de saída
- [ ] 11. Ids estáveis
- [ ] 12. Busca com teste
- [ ] 13. Scripts npm documentados
- [ ] 14. Pasta organizada
- [ ] 15. GitHub sem `node_modules` e sem o JSON pessoal
- [ ] 16. Opcional: desfazer

## Próximo passo

Siga para [07 · Async e await](../07-async-e-await/README.md). A CLI continua; o que muda agora é o tempo: quando o programa espera o disco e quando ele segue.
