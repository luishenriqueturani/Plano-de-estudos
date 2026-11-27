# Sessão 20 — Tarefas agendadas

**Sessão 20 de 27 · cerca de 1 semana**

Tem trabalho que ninguém dispara clicando: lembrar item parado em "fazendo", apagar rascunho velho, contar quantos itens nasceram na semana. O Nest roda isso com `@nestjs/schedule`, no mesmo processo da API. A expressão cron é o calendário.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [19 · Migrations e seeders](../19-migrations-e-seeders/README.md)
> **Próxima →** [21 · Workers, filas e Kafka](../21-workers-filas-e-kafka/README.md)

## O que você vai saber fazer

- Registrar o agendador uma única vez.
- Ler uma expressão cron campo a campo.
- Chamar um service a partir do job, sem copiar regra para dentro do cron.
- Ver no log que rodou, e o que acontece se lançar erro.

## Documentação

- [Task scheduling](https://docs.nestjs.com/application/task-scheduling)
- A página de [módulos](https://docs.nestjs.com/modules), para o job morar num módulo em vez de no `AppModule` inchado

A documentação mostra expressões de seis campos (com segundos). Use o formato que a página descrever no dia em que você ler. Não misture com o cron de cinco campos do sistema operacional sem contar os campos.

## Tarefas

### Tarefa 1 — Um trabalho com motivo

**Faça:** escolha uma rotina da Estante. Sugestão: itens em `fazendo` sem atualização há mais de N dias passam a ter um marcador `parado`, ou geram uma linha de log com a contagem. O marcador precisa da coluna que você criou na sessão 19, ou de uma migration nova. Escreva a regra em três linhas antes de instalar o pacote.

**Pronto quando:** dá para testar a regra chamando o service na mão, sem cron.

### Tarefa 2 — Instalar e registrar uma vez

**Leia:** [task scheduling](https://docs.nestjs.com/application/task-scheduling), inclusive o aviso de chamar `forRoot()` em um módulo só.

**Faça:** instale `@nestjs/schedule`, importe `ScheduleModule.forRoot()` só no módulo raiz. Crie `TarefasModule` com um service.

**Pronto quando:** o app sobe e você sabe apontar o único `forRoot`.

### Tarefa 3 — Job de depuração

**Faça:** um método com `@Cron` a cada minuto, que só escreve no `Logger`. Espere dois minutos e confira que saiu duas vezes, não quatro. Se sair quatro, o módulo foi importado em duplicata: corrija antes de seguir.

**Pronto quando:** o log comprova a frequência.

### Tarefa 4 — Traduzir expressões

**Faça:** no README, traduza para português estas expressões, conferindo a ordem dos campos na documentação:

- a cada minuto, no segundo 0;
- todo dia às 7h;
- de segunda a sexta, ao meio-dia;
- a cada 15 minutos.

Troque o job de depuração para uma delas só enquanto você observa, depois volte para a expressão da rotina real. A de todo minuto não fica no projeto final desta sessão: acorda o banco à toa.

**Pronto quando:** as quatro traduções estão escritas e uma delas foi observada no log.

### Tarefa 5 — A rotina de verdade

**Faça:** o cron chama um método do service de itens (ou de um service de manutenção). A regra da tarefa 1 mora nesse método, com teste unitário. O método do cron é fino: chama e registra quantos itens foram afetados.

**Pronto quando:** o teste da regra passa sem relógio, e o cron, apontando para um intervalo curto durante o teste manual, altera o banco como a regra descreve.

### Tarefa 6 — Erro dentro do job

**Leia:** o que a documentação diz quando o método lança exceção.

**Faça:** force um erro uma vez (consulta com tabela errada, num branch de experimento). Veja o log. Corrija. Confirme que o processo do Nest continuou no ar e o próximo horário ainda dispara.

**Pronto quando:** você descreve isso no README em quatro linhas. O experimento não fica na branch principal.

### Tarefa 7 — Não acoplar o HTTP

**Faça:** não existe rota "rodar cron" pública. Se você quiser disparar na mão durante o desenvolvimento, um método do service chamado por um teste basta. Rota administrativa sem autenticação fica para nunca: a sessão 22 ainda nem começou.

**Pronto quando:** `docs/rotas.md` não ganhou endpoint de manutenção aberto.

### Tarefa 8 — README

**Faça:** a expressão final, o que ela faz na Estante, e como observar (olhar o log, olhar a coluna no `psql`).

**Pronto quando:** outra pessoa descobre o horário sem ler o decorador.

## Checklist

- [ ] 1. Regra testável sem cron
- [ ] 2. `forRoot` uma vez
- [ ] 3. Frequência conferida no log
- [ ] 4. Quatro expressões traduzidas
- [ ] 5. Job chama o service
- [ ] 6. Erro não mata o processo
- [ ] 7. Sem rota pública de disparo
- [ ] 8. README com o horário em português

## Próximo passo

Siga para [21 · Workers, filas e Kafka](../21-workers-filas-e-kafka/README.md). O cron desta sessão marca a hora. A fila é quem passa a fazer o trabalho.
