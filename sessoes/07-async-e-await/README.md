# Sessão 07 — Async e await

**Sessão 7 de 27 · cerca de 3 semanas**

Na sessão 06 o arquivo já era gravado com `async/await`, como quem usa uma ferramenta sem abrir o manual. Esta sessão abre o manual. Uma operação de disco, de rede ou de banco não termina na linha seguinte. O programa precisa dizer o que fazer quando o resultado chegar, e o que fazer quando falhar.

A CLI da Estante continua sendo o projeto. O que muda é a forma de esperar.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [06 · Node.js e lógica](../06-nodejs-e-logica/README.md)
> **Próxima →** [08 · Concorrência e paralelismo](../08-concorrencia-e-paralelismo/README.md)

## O que você vai saber fazer

- Explicar o que uma Promise representa.
- Escrever o mesmo fluxo com `.then()` e com `async/await`.
- Tratar erro com `try/catch` em volta do `await`.
- Escolher entre esperar em sequência e esperar várias coisas juntas.
- Apontar um `await` esquecido.

## Documentação

- [JavaScript assíncrono](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Async_JS/Introducing)
- [Promises](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Async_JS/Promises)
- [async e await na MDN](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Async_JS) — o artigo de `async`/`await` está neste módulo; se o atalho mudar, abra o índice e siga a ordem
- [`async function`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/async_function)
- [Event loop, timers e `nextTick`](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [`fs/promises`](https://nodejs.org/api/fs.html)

## Tarefas

Continue em `estante-cli`.

### Tarefa 1 — O que ainda não aconteceu

**Leia:** [Introducing asynchronous JavaScript](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Async_JS/Introducing).

**Faça:** no README, com as suas palavras, a diferença entre uma função que devolve o texto do arquivo e uma função que devolve a promessa desse texto. Use a CLI como exemplo.

**Pronto quando:** as duas frases existem antes de você mudar código.

### Tarefa 2 — Uma espera visível

**Faça:** uma função `esperar(ms)` que devolve uma Promise resolvida com `setTimeout`. Um script imprime `antes`, espera um segundo, imprime `depois`.

**Pronto quando:** a ordem no terminal é `antes`, pausa, `depois`, e você consegue dizer em que momento o `await` devolveu a vez.

### Tarefa 3 — Do `.then()` para o `await`

**Leia:** [Promises](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Async_JS/Promises).

**Faça:** leia um arquivo pequeno duas vezes no mesmo script. A primeira versão usa `.then()`. A segunda usa `async/await` e faz a mesma coisa. As duas imprimem o conteúdo.

**Pronto quando:** as duas versões estão no repositório, mesmo que você apague a de `.then()` no fim e deixe um comentário de uma linha dizendo por que ficou com `await`.

### Tarefa 4 — Erro não é impressão no meio do caminho

**Faça:** `await` num arquivo que não existe, dentro de `try/catch`. A mensagem que a pessoa vê é a sua. O processo termina com código diferente de zero. Sem `try/catch`, observe o erro de Promise rejeitada e anote a diferença.

**Pronto quando:** os dois comportamentos estão descritos no README, e a versão final é a que tem `catch`.

### Tarefa 5 — A CLI inteira em funções async

**Faça:** `carregar` e `salvar` continuam async. Quem chama também é async, até o ponto de entrada. Não misture `.then()` no meio do comando `adicionar`.

**Pronto quando:** `node estante.js listar` funciona e nenhum comando ignora a Promise (um aviso de Promise não tratada no terminal é falha desta tarefa).

### Tarefa 6 — Sequência tem ordem

**Faça:** três `esperar` em sequência, cada um imprimindo o próprio número. O total leva a soma dos tempos. Meça de um jeito simples (`Date.now()` antes e depois).

**Pronto quando:** o README registra os tempos que você viu e diz por que o segundo não começou junto com o primeiro.

### Tarefa 7 — `Promise.all` quando os trabalhos não dependem um do outro

**Leia:** `Promise.all` na documentação de Promise.

**Faça:** leia três arquivos ao mesmo tempo com `Promise.all`. Compare o tempo com a versão em sequência. Se um dos caminhos não existir, o `all` rejeita: mostre isso de propósito e anote.

**Pronto quando:** os dois tempos estão no README e o caso do arquivo ausente tem `catch`.

### Tarefa 8 — Uma falha não apaga as outras

**Leia:** `Promise.allSettled`.

**Faça:** o mesmo trio de arquivos, agora com `allSettled`. Imprima, para cada um, se deu certo ou qual foi o erro. Os que existem são lidos mesmo quando um falta.

**Pronto quando:** a saída mostra os três resultados, e você escreve em uma frase quando usaria `all` e quando usaria `allSettled` na Estante.

### Tarefa 9 — `await` dentro de laço

**Faça:** grave cinco itens, um `await salvar` por volta do laço, e depois uma versão que junta os trabalhos independentes. Na CLI real, gravar o JSON a cada item precisa da versão anterior do arquivo: esse caso fica em sequência, e o README explica por quê. Não "otimize" a gravação do mesmo arquivo em paralelo.

**Pronto quando:** o README tem o caso que é paralelo e o caso que precisa ser serial, os dois com um motivo.

### Tarefa 10 — O `await` esquecido

**Faça:** tire o `await` de um `salvar` de propósito. Rode o comando duas vezes rápido, ou encerre o processo logo em seguida, e veja o arquivo ficar para trás ou o aviso de Promise. Recoloque o `await`.

**Pronto quando:** o README conta o que quebrou, com a saída que você viu.

### Tarefa 11 — Teste de uma função async

**Faça:** um teste que espera a função de carregar. Use o estilo async que o test runner do Node aceita na documentação de `node:test`. O teste falha se você esquecer de esperar a asserção.

**Pronto quando:** `npm test` passa, e um `expect` no valor errado falha de verdade, não "passa" porque a asserção ficou para depois do fim do teste.

### Tarefa 12 — O que o event loop ainda não é

**Leia:** o começo de [event loop, timers e nextTick](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick). Pare antes de decorar a tabela inteira de fases.

**Faça:** escreva quatro linhas: o Node consegue esperar arquivo e timer sem uma thread por espera. Um cálculo pesado no meio ainda segura o processo. A sessão 08 separa esses dois mundos.

**Pronto quando:** essas linhas estão no README, com um exemplo seu, não com o texto da documentação colado.

## Checklist

- [ ] 1. Promise explicada com a CLI
- [ ] 2. `esperar` com ordem visível
- [ ] 3. `.then()` e `await` no mesmo fluxo
- [ ] 4. Arquivo ausente tratado
- [ ] 5. Comandos async sem Promise solta
- [ ] 6. Esperas em sequência medidas
- [ ] 7. `Promise.all` com uma falha
- [ ] 8. `allSettled` comparado ao `all`
- [ ] 9. Laço serial e laço paralelo, cada um no seu caso
- [ ] 10. `await` esquecido, observado e corrigido
- [ ] 11. Teste que realmente espera
- [ ] 12. Limite do event loop escrito para a próxima sessão

## Próximo passo

Siga para [08 · Concorrência e paralelismo](../08-concorrencia-e-paralelismo/README.md).
