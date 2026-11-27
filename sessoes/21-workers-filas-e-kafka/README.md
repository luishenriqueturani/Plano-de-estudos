# Sessão 21 — Workers, filas e Kafka

**Sessão 21 de 27 · cerca de 3 semanas**

O cron da sessão 20 acorda dentro do mesmo processo da API. Isso serve para um lembrete leve. Deixa de servir quando o trabalho demora, falha no meio, ou precisa sobreviver a um restart: enviar e-mail, gerar um relatório, avisar outro sistema. Aí o pedido HTTP só registra o trabalho numa fila. Outro processo, o worker, executa.

Kafka entra na segunda metade. Não é "uma fila mais famosa". É um registro de eventos que vários consumidores podem ler, cada um no próprio ritmo. Você vai publicar um fato da Estante e ler esse fato em outro processo. A sessão 08 pediu essa distinção. Aqui ela vira código.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [20 · Tarefas agendadas](../20-cron-jobs/README.md)
> **Próxima →** [22 · Segurança do backend](../22-seguranca-backend/README.md)

## O que você vai saber fazer

- Subir Redis e um worker BullMQ ao lado do Nest.
- Enfileirar trabalho no cron e na API, em vez de executá-lo no pedido.
- Ver retry quando o trabalho falha.
- Subir um Kafka local e publicar um evento da Estante.
- Explicar, com o seu código, quando a fila basta e quando o registro de eventos passa a valer a pena.

## Documentação

- [BullMQ](https://docs.bullmq.io/)
- [Filas](https://docs.bullmq.io/guide/queues)
- [Workers](https://docs.bullmq.io/guide/workers)
- [Falhas e novas tentativas](https://docs.bullmq.io/guide/retrying-failing-jobs)
- [Imagem do Redis](https://hub.docker.com/_/redis)
- [Começar com o Kafka](https://kafka.apache.org/quickstart)
- [Introdução da documentação do Kafka](https://kafka.apache.org/documentation/#gettingStarted)
- A sessão [08](../08-concorrencia-e-paralelismo/README.md) e o cron da sessão [20](../20-cron-jobs/README.md)

BullMQ é a fila deste plano porque fala JavaScript e usa Redis, que você sobe com o Docker que já tem. O conceito (produtor, trabalho, worker, tentativa) aparece em outras ferramentas com outros nomes.

## Tarefas

Continue em `estante-nest`. O worker pode ser outro arquivo de entrada no mesmo repositório (`worker.ts`), rodando com outro comando. Não embuta o processamento pesado no `main` da API.

### Tarefa 1 — Três palavras

**Faça:** no README, defina produtor, fila e worker com a rotina da sessão 20. O cron é produtor. O worker faz a marcação de item parado, ou o trabalho que você escolheu. A API continua respondendo enquanto o worker trabalha.

**Pronto quando:** o desenho tem dois processos Node, não um.

### Tarefa 2 — Redis no Compose

**Leia:** a [imagem oficial do Redis](https://hub.docker.com/_/redis) o bastante para subir com senha local ou, nesta rede de estudo, sem expor a porta para além da sua máquina. Não publique `6379` para a internet. No Compose de desenvolvimento, publicar no localhost pode facilitar o teste: escreva o que você fez.

**Faça:** serviço `redis` no `compose.yaml`. A API lê a URL por variável de ambiente.

**Pronto quando:** `docker compose up -d` mostra o Redis de pé e a API só conecta se a URL estiver certa.

### Tarefa 3 — Enfileirar e processar

**Leia:** [queues](https://docs.bullmq.io/guide/queues) e [workers](https://docs.bullmq.io/guide/workers).

**Faça:** uma fila `estante`. Um endpoint de desenvolvimento, ou um método de teste, adiciona um job `{ itemId }`. O worker, noutro terminal, marca o item ou escreve um log com o id. Os dois processos usam o mesmo Redis.

**Pronto quando:** você derruba o worker, enfileira, sobe o worker de novo, e o job não se perdeu.

### Tarefa 4 — O cron só enfileira

**Faça:** a rotina da sessão 20 deixa de fazer o trabalho pesado no processo da API. No horário, ela consulta quem precisa de atenção e adiciona um job por item (ou um job com a lista, se você justificar). O worker executa a regra que já tinha teste.

**Pronto quando:** o teste da regra continua sem Redis, e o caminho do cron até o banco passa pela fila. O README descreve os dois comandos: API e worker.

### Tarefa 5 — Falha e tentativa

**Leia:** a página de retry do BullMQ (busque "retry" na documentação se o atalho acima mudar).

**Faça:** force o worker a falhar nas duas primeiras vezes (um contador ou um erro proposital) e acertar na terceira, dentro do limite que você configurou. Job que esgota as tentativas fica visível como falho. Anote onde você olhou (comando, log ou interface que a documentação indicar).

**Pronto quando:** o README tem o número de tentativas e o que acontece depois da última.

### Tarefa 6 — O pedido HTTP não espera o worker

**Faça:** um `POST` que só enfileira responde 202 com o id do job, não com o resultado do trabalho. Se o seu caso for "criar item", o item continua sendo gravado na hora. O que vai para a fila é o extra (um aviso, um recálculo). Não existe tela ainda para o push: o worker pode só registrar que "avisaria".

**Pronto quando:** a resposta volta com o worker desligado, e o trabalho acontece quando o worker sobe.

### Tarefa 7 — Dois consumidores não são a mesma fila

**Faça:** escreva, antes do Kafka, o limite que você já sente: um job da fila é pego por um worker. Se amanhã um e-mail e uma estatística precisarem do mesmo fato, você ou enfileira dois jobs, ou muda de ferramenta. Essa frase é a porta do Kafka.

**Pronto quando:** a frase está no README, com as suas palavras.

### Tarefa 8 — Kafka local

**Leia:** o [quickstart](https://kafka.apache.org/quickstart) do começo até produzir e consumir uma mensagem. Use o Compose ou os comandos que a página mostrar para a versão atual. KRaft, sem ZooKeeper, se for o que o quickstart usar.

**Faça:** suba o broker. Crie o tópico `estante.itens`. Produza uma mensagem de teste com a ferramenta de linha de comando da documentação. Consuma essa mensagem.

**Pronto quando:** você viu o texto que enviou aparecer no consumidor, e o broker está no `compose.yaml` junto do Redis, ou num compose separado documentado. A porta do broker não fica aberta para a internet.

### Tarefa 9 — A API publica um fato

**Faça:** ao criar um item, além de gravar no Postgres, publique no tópico uma mensagem pequena: id, título, instante. Use o cliente Kafka para Node que a documentação do ecossistema indicar no momento (`kafkajs` é o habitual; confira se o quickstart ou o site do cliente ainda recomenda esse). Segredo e broker vêm de variável de ambiente.

**Pronto quando:** criar um item pela API faz a mensagem aparecer num consumidor de terminal, sem o worker da fila BullMQ no meio.

### Tarefa 10 — Outro processo lê o mesmo fato

**Faça:** um script `consumidor.ts` que só registra a mensagem. Suba dois consumidores em grupos diferentes, se a documentação de consumer group estiver ao alcance, ou leia essa página e explique com um desenho: no mesmo grupo, a mensagem vai para um; em grupos diferentes, os dois recebem. Implemente o caso dos dois grupos se der tempo. Se travar mais de um encontro, o desenho mais um consumidor só já fecham a tarefa, e o README diz o que ficou de fora.

**Pronto quando:** o README distingue "fila: um worker pega o job" de "tópico: cada grupo interessado lê o fato".

### Tarefa 11 — O que não entra no tópico

**Faça:** a senha, o hash e o token não vão na mensagem. O corpo leva id e o mínimo para o consumidor buscar o resto na API ou no banco, se precisar. Escreva essa regra.

**Pronto quando:** você olhou o JSON publicado e ele passa na regra.

### Tarefa 12 — Comparar no seu sistema

**Faça:** uma tabela com a rotina de item parado, o aviso futuro e o fato "item criado".

| Trabalho | Onde ficou | Por quê |
| --- | --- | --- |
| Item parado | fila | uma execução, com retry |
| Item criado | tópico | mais de um interessado pode aparecer |
| Marcar horário | cron | só decide quando olhar |

Ajuste os nomes ao que você realmente ligou.

**Pronto quando:** as três linhas citam arquivo ou comando seu.

## Checklist

- [ ] 1. Produtor, fila e worker desenhados
- [ ] 2. Redis no Compose
- [ ] 3. Job sobrevive ao worker desligado
- [ ] 4. Cron só enfileira
- [ ] 5. Retry com falha proposital
- [ ] 6. HTTP responde sem esperar o trabalho extra
- [ ] 7. Limite da fila escrito
- [ ] 8. Kafka local produz e consome
- [ ] 9. A API publica "item criado"
- [ ] 10. Consumidor separado, grupos explicados
- [ ] 11. Mensagem sem segredo
- [ ] 12. Tabela cron, fila e tópico

## Próximo passo

Siga para [22 · Segurança do backend](../22-seguranca-backend/README.md). A fila e o tópico ficam de pé: o e-mail da sessão 24 vai entrar como job, não como chamada no meio do login.
