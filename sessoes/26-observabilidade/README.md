# Sessão 26 — Observabilidade com Prometheus e Grafana

**Sessão 26 de 27 · cerca de 3 semanas**

Subir a stack, na sessão 25, responde "está no ar?". Não responde "quantos logins falharam na última hora?" nem "qual pedido quebrou e por quê?". Observabilidade, neste plano, são as duas respostas: número ao longo do tempo, e o registro daquele pedido.

Prometheus guarda as métricas. A API expõe um endereço que ele visita. Grafana desenha. O log sai em JSON, uma linha por fato, com um id que amarra as linhas do mesmo pedido. Rastreio distribuído, o desenho do caminho entre Nginx, Nest e banco, fica de fora: o README diz o nome (trace) sem implementar.

O endereço de métricas não passa pelo Nginx público. Só a rede do Compose, onde o Prometheus está, enxerga esse caminho.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [25 · Deploy com Docker e Nginx](../25-deploy-docker-nginx/README.md)
> **Próxima →** [27 · Projeto final](../27-projeto-final/README.md)

## O que você vai saber fazer

- Expor contadores e um histograma de duração na API Nest.
- Fazer o Prometheus coletar isso dentro do Compose.
- Montar um painel no Grafana com uma pergunta real sobre a Estante.
- Deixar `/metrics` fora da porta que o navegador usa para o site.
- Emitir log em JSON, com nível, horário e o id do pedido.
- Distinguir métrica, log e trace em uma frase cada.

## Documentação

- [Visão geral do Prometheus](https://prometheus.io/docs/introduction/overview/)
- [Começar com o Prometheus](https://prometheus.io/docs/prometheus/latest/getting_started/)
- [Começar com o Grafana](https://grafana.com/docs/grafana/latest/getting-started/)
- [prom-client](https://github.com/siimon/prom-client) — o cliente de métricas para Node
- [Configuração de scrape](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
- [Logger do Nest](https://docs.nestjs.com/techniques/logger)
- [Pino](https://getpino.io/) — log JSON para Node. No Nest, o caminho usual é o [nestjs-pino](https://github.com/iamolegga/nestjs-pino)
- A stack da sessão [25](../25-deploy-docker-nginx/README.md)

## Tarefas

Trabalhe no Compose da sessão 25 e no código do Nest. Grafana e Prometheus entram como serviços novos, não no lugar do Nginx.

### Tarefa 1 — Três perguntas

**Faça:** escreva três perguntas que um número consegue responder sobre a sua Estante. Exemplos que prestam: quantos itens foram criados, quantos logins falharam, quanto tempo o `GET /itens` leva. "Está tudo bem?" não é pergunta de métrica até você definir o número.

**Pronto quando:** as três perguntas estão no README, e pelo menos duas dá para contar no código que você já tem.

### Tarefa 2 — Métrica, log e trace

**Leia:** a visão geral do [Prometheus](https://prometheus.io/docs/introduction/overview/), o bastante para separar contador de medição instantânea.

**Faça:** uma frase para cada palavra.

- Métrica: a série de números que o Prometheus vai guardar.
- Log: a linha JSON de um fato, com horário e id do pedido. `docker compose logs` continua sendo o lugar onde você lê isso. A forma da linha muda nas tarefas de log desta sessão.
- Trace: o caminho de um pedido único entre Nginx, Nest e banco. Você não vai construir isso agora.

**Pronto quando:** as três frases usam a Estante como exemplo.

### Tarefa 3 — Contadores na API

**Leia:** o README do [prom-client](https://github.com/siimon/prom-client), as partes de `Counter`, `Histogram` e o registro padrão.

**Faça:** um `GET /metrics` no Nest devolve o texto que o Prometheus entende. Conte pedidos HTTP por método, rota e status. Meça a duração com um histograma. Não use o id do item como rótulo: isso cria uma série por item e estoura a memória do Prometheus com o tempo. Rótulos são método, rota e status.

**Pronto quando:** `curl` no `/metrics`, de dentro da rede ou da porta que só você usa em desenvolvimento, mostra o contador subindo depois de alguns pedidos.

### Tarefa 4 — O Nginx não publica isso

**Faça:** confira a configuração da sessão 25. `/metrics` não está no `location` que vai para a internet. O Prometheus alcança a API pelo nome do serviço na rede do Compose (`api:3001/metrics`, ou a porta real). Se em desenvolvimento você publicou a porta da API no localhost para teste, o README diz para tirar essa publicação quando a stack for a da sessão 25.

**Pronto quando:** abrir `http://localhost:8080/metrics` no navegador não mostra os números. O Prometheus, sim, consegue ler.

### Tarefa 5 — Prometheus no Compose

**Leia:** [getting started](https://prometheus.io/docs/prometheus/latest/getting_started/) e a página de [configuração](https://prometheus.io/docs/prometheus/latest/configuration/configuration/), a parte de `scrape_configs`.

**Faça:** serviço `prometheus` com um arquivo `prometheus.yml` montado no container. `scrape_interval` de 15s basta. O alvo é a API. A interface do Prometheus, nesta etapa de estudo, pode ficar em `localhost` numa porta alta, sem ir para o Nginx público.

**Pronto quando:** a página de alvos do Prometheus mostra o job da API como saudável, e uma consulta `up` devolve 1.

### Tarefa 6 — Uma consulta que responde a tarefa 1

**Faça:** no explorador do Prometheus, escreva a consulta da pergunta mais importante (taxa de erro ou contagem de itens criados, o que você instrumentou). Se a criação de item não passou pelo contador HTTP, use a rota `POST` correspondente. Anote a consulta no README.

**Pronto quando:** o gráfico mexe quando você usa a Estante, e fica quieto quando você não usa.

### Tarefa 7 — Grafana em cima

**Leia:** [getting started do Grafana](https://grafana.com/docs/grafana/latest/getting-started/) até adicionar o Prometheus como fonte e criar um painel.

**Faça:** serviço `grafana` no Compose, senha de admin por variável de ambiente, não `admin/admin` commitado. Fonte de dados apontando para o serviço `prometheus`. Um painel com a consulta da tarefa 6 e outro com a duração do histograma, se você criou o histograma.

**Pronto quando:** o painel abre em `localhost` e mostra dado de verdade, não o painel de exemplo vazio.

### Tarefa 8 — O que não vira rótulo

**Faça:** procure nos rótulos e-mail, id de usuário e título de item. Não devem estar lá. Se estiverem, troque por algo de baixa cardinalidade (rota, status, fila). Escreva a regra no README: rótulo é categoria, não identificador de pessoa.

**Pronto quando:** a regra está escrita e a saída de `/metrics` passa nela.

### Tarefa 9 — Volume e restart

**Faça:** o Grafana guarda o painel num volume, senão o próximo `up` esquece o desenho. Reinicie os containers sem `-v` e abra o painel de novo. O Prometheus pode perder a janela curta de estudo se você não der volume a ele: decida, documente, e não prometa histórico que o volume não segura.

**Pronto quando:** o painel continua lá depois do restart, e o README diz por quanto tempo o Prometheus guarda amostra, se você configurou retenção, ou que está no padrão da imagem.

### Tarefa 10 — Uma linha de texto não acha o pedido

**Faça:** no README, duas linhas imaginadas do log atual, do tipo `Erro ao criar item` e `select falhou`, sem horário comum e sem id. Escreva por que, com dois pedidos ao mesmo tempo, você não sabe qual frase pertence a qual pessoa.

**Pronto quando:** o exemplo está escrito e a limitação está numa frase sua.

### Tarefa 11 — Log em JSON

**Leia:** [Logger do Nest](https://docs.nestjs.com/techniques/logger) e o [Pino](https://getpino.io/). Para encaixar no Nest, siga o [nestjs-pino](https://github.com/iamolegga/nestjs-pino) na versão que você instalar.

**Faça:** a API passa a escrever uma linha JSON por evento, com pelo menos horário, nível (`info`, `warn`, `error`) e mensagem. Criar um item gera uma linha `info`. Um erro gera `error`. Em desenvolvimento você pode deixar o Pino colorido, se a documentação mostrar o modo pretty. No container da sessão 25, a saída fica em JSON puro, que é o que uma ferramenta de log consegue filtrar.

**Pronto quando:** `docker compose logs api` (ou o terminal do `npm run dev`) mostra um objeto com esses campos, e um `console.log` solto que ainda restava foi trocado pelo logger.

### Tarefa 12 — O mesmo id em todo o pedido

**Faça:** cada requisição ganha um identificador. O logger inclui esse id em todas as linhas daquela requisição. A resposta pode devolver o id num cabeçalho (`x-request-id` serve), para você copiar da aba Rede do navegador e buscar no log.

Provoque dois pedidos juntos. Filtre o log por um dos ids e confira que as linhas do outro não aparecem.

**Pronto quando:** o README mostra um id real e as duas ou três linhas que ele amarrou.

### Tarefa 13 — O que não entra no log

**Faça:** revise as chamadas do logger. Senha, hash, token de sessão, chave do Firebase e corpo cru do login não aparecem. Pode logar o id do usuário e a rota. Erro inesperado loga a causa no servidor. A resposta HTTP continua genérica, como na sessão 22.

**Pronto quando:** você provocou um login com senha errada e a linha de log não contém a senha.

### Tarefa 14 — Quantos no gráfico, por quê no log

**Faça:** provoque um erro 500. O gráfico de status mostra a contagem. Com o id do cabeçalho, o log mostra a causa daquele pedido. Escreva no README qual dos dois você abre primeiro para "quantos" e qual você abre primeiro para "por quê".

**Pronto quando:** as duas frases citam o id que você usou e o que cada ferramenta mostrou.

## Checklist

- [ ] 1. Três perguntas numéricas
- [ ] 2. Métrica, log e trace
- [ ] 3. `/metrics` com contador e duração
- [ ] 4. Nginx público sem esse caminho
- [ ] 5. Alvo saudável no Prometheus
- [ ] 6. Consulta que responde uma pergunta
- [ ] 7. Painel no Grafana
- [ ] 8. Rótulo sem dado de pessoa
- [ ] 9. Painel sobrevive ao restart
- [ ] 10. Limite do log sem id, escrito
- [ ] 11. Linha JSON com nível, horário e mensagem
- [ ] 12. Um id amarra as linhas de um pedido
- [ ] 13. Senha fora do log
- [ ] 14. "Quantos" no gráfico, "por quê" no log daquele id

## Próximo passo

Siga para [27 · Projeto final](../27-projeto-final/README.md). Se o produto final tiver uma fila, um aviso ou uma métrica, reaproveite estas sessões em vez de inventar outro caminho. Não precisa repetir Kafka, Firebase e Grafana inteiros: a rubrica pede pelo menos um desses fios.
