# Sessão 08 — Concorrência e paralelismo

**Sessão 8 de 28 · cerca de 2 semanas**

Async e await fizeram o Node esperar arquivo e timer sem congelar a aparência do código. Isso é concorrência: várias coisas em andamento, uma thread executando o seu JavaScript. Paralelismo é outra coisa: duas partes do trabalho andando ao mesmo tempo, em threads ou processos diferentes.

Fila, worker de verdade e Kafka ficam para a sessão 22, quando já existir uma API. Aqui o objetivo é o conceito, com um experimento pequeno em cada lado.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [07 · Async e await](../07-async-e-await/README.md)
> **Próxima →** [09 · SQLite](../09-sqlite/README.md)

## O que você vai saber fazer

- Definir concorrência e paralelismo com um exemplo da Estante.
- Mostrar o event loop travado por um cálculo.
- Mostrar o event loop livre enquanto um timer ou um arquivo espera.
- Rodar um cálculo pesado num worker thread.
- Dizer qual ferramenta futura (fila, processo separado, Kafka) resolve qual tipo de trabalho.

## Documentação

- [Não bloqueie o event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop)
- [Event loop, timers e `nextTick`](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [`worker_threads`](https://nodejs.org/api/worker_threads.html)
- A sessão [07](../07-async-e-await/README.md), o seu próprio resumo

## Tarefas

Pode ser uma pasta `lab-concorrencia` dentro de `estante-cli`, ou um repositório `estante-concorrencia`. Os experimentos não precisam entrar no comando `estante.js`.

### Tarefa 1 — Duas definições

**Faça:** no README, duas definições curtas e um exemplo cada.

- Concorrência: a CLI espera o disco e, no mesmo processo, um timer ainda pode disparar.
- Paralelismo: contar palavras de um arquivo enorme em outra thread, enquanto a thread principal segue respondendo.

**Pronto quando:** os exemplos são da Estante, não de um servidor genérico copiado de artigo.

### Tarefa 2 — O laço que segura tudo

**Leia:** [Don't block the event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop), a parte que explica trabalho de CPU.

**Faça:** um script que arma um `setInterval` de 200 ms imprimindo `tick`, e no meio roda um laço pesado (uma soma grande, o bastante para levar cerca de um segundo). Observe os ticks pararem durante o laço.

**Pronto quando:** a saída mostra o buraco, e o README diz que `async/await` não teria ajudado: não havia operação de espera, havia conta.

### Tarefa 3 — A espera que não segura

**Faça:** o mesmo `setInterval`, trocando o laço por `await esperar(1000)`. Os ticks continuam durante o segundo.

**Pronto quando:** você coloca as duas saídas lado a lado no README.

### Tarefa 4 — Uma frase sobre o event loop

**Leia:** o suficiente de [event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick) para não tratar timer e arquivo como threads.

**Faça:** escreva: o JavaScript do Node roda numa thread. O disco e a rede avisam quando terminam. Enquanto o seu código ocupa essa thread, o aviso espera.

**Pronto quando:** a frase está no README e não cita "o Node é multithread" sem dizer de qual parte você fala.

### Tarefa 5 — Worker thread

**Leia:** o exemplo mínimo de [worker_threads](https://nodejs.org/api/worker_threads.html) (`Worker`, `parentPort`, mensagem de ida e volta).

**Faça:** o laço pesado da tarefa 2 vai para um worker. A thread principal continua imprimindo `tick`. O worker devolve o resultado com `postMessage`. A principal imprime o resultado quando a mensagem chega.

**Pronto quando:** os ticks não param, e o número que volta é o mesmo do laço que rodava na thread principal.

### Tarefa 6 — O que não vai para o worker

**Faça:** liste três trabalhos da Estante e diga onde cada um mora.

- Ler o JSON ou, mais tarde, consultar o banco: espera de entrada e saída, `async/await` na thread principal.
- Gerar um relatório enorme, varrer milhares de títulos com conta pesada: worker ou processo separado.
- Mandar um e-mail daqui a alguns meses: nem worker thread de brinquedo. Uma fila, sessão 22.

**Pronto quando:** a lista está no README.

### Tarefa 7 — Processo é outra fronteira

**Faça:** leia na documentação do Node o bastante sobre processo filho para explicar, sem implementar um servidor, a diferença entre thread do `worker_threads` e outro processo (`node outro.js`). Uma frase: processo tem memória separada; thread do worker também não divide objeto, a conversa é por mensagem. Você não precisa subir um cluster.

**Pronto quando:** o README tem essa comparação em cinco linhas, e você não afirmou que dois workers enxergam o mesmo array.

### Tarefa 8 — O desenho

**Faça:** um desenho com três caixas: thread principal, espera de disco, worker de CPU. Setas só onde existe mensagem ou conclusão. Kafka e Redis ainda não aparecem. Deixe um espaço vazio com o rótulo "fila, sessão 22".

**Pronto quando:** o desenho está no README e bate com as tarefas 2, 3 e 5.

### Tarefa 9 — Onde isso volta no plano

**Faça:** uma tabela de quatro linhas: cron (sessão 21), fila com worker (sessão 22), e-mail (sessão 25), métrica (sessão 27). Em cada linha, "concorrência", "paralelismo" ou "os dois", com uma razão. É previsão: você confirma de novo quando chegar lá. Se a previsão mudar, você atualiza.

**Pronto quando:** a tabela existe e nenhuma linha diz "tanto faz".

## Checklist

- [ ] 1. Definições com exemplo da Estante
- [ ] 2. Laço de CPU segura os ticks
- [ ] 3. `await` não segura os ticks
- [ ] 4. Uma thread de JavaScript, esperas por fora
- [ ] 5. Worker devolve o resultado e os ticks seguem
- [ ] 6. Três trabalhos, três lugares
- [ ] 7. Thread e processo comparados
- [ ] 8. Desenho
- [ ] 9. Tabela do que ainda vem

## Próximo passo

Siga para [09 · SQLite](../09-sqlite/README.md). O tempo fica de lado por umas semanas. O próximo problema é guardar linha, não esperar arquivo.
