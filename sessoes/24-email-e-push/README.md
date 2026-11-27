# Sessão 24 — E-mail e notificações push

**Sessão 24 de 27 · cerca de 2 semanas**

A Estante já sabe quem é a pessoa e já tem uma fila. Falta avisar. São dois caminhos, de propósito diferentes.

O e-mail sai por SMTP, com o Nodemailer, dentro de um job. O Firebase Cloud Messaging não envia e-mail da aplicação. O push, esse sim, é o Firebase: o navegador entrega um token, o Nest guarda, o worker pede ao Firebase para entregar a notificação. Existe extensão do Firebase que dispara e-mail a partir do Firestore. Este plano não troca o Postgres por isso.

Nenhum dos dois avisos bloqueia o pedido HTTP. Os dois passam pela fila da sessão 21.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [23 · Segurança do frontend](../23-seguranca-frontend/README.md)
> **Próxima →** [25 · Deploy com Docker e Nginx](../25-deploy-docker-nginx/README.md)

## O que você vai saber fazer

- Enviar um e-mail de estudo para uma caixa que não é a de uma pessoa real.
- Colocar esse envio num job, com retry.
- Registrar um projeto Firebase e obter um token de push no Next.
- Mandar uma notificação para esse token a partir do Nest, sem commitar a chave da conta de serviço.
- Dizer o que o usuário vê se recusar a permissão, e o que a API faz nesse caso.

## Documentação

- [Nodemailer](https://nodemailer.com/)
- [Filas do BullMQ](https://docs.bullmq.io/guide/queues), de novo: o envio é um job
- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)
- [FCM no navegador](https://firebase.google.com/docs/cloud-messaging/js/client)
- [Enviar pelo Admin SDK](https://firebase.google.com/docs/cloud-messaging/server)
- [Configurar o Admin SDK](https://firebase.google.com/docs/admin/setup)
- A sessão [21](../21-workers-filas-e-kafka/README.md) e a conta da sessão [23](../23-seguranca-frontend/README.md)

Para o SMTP de estudo, o próprio Nodemailer documenta uma conta de teste (Ethereal) na página de uso. Mailtrap ou outro capturador serve do mesmo jeito, se você ler a documentação dele. A meta é ver a mensagem numa caixa de desenvolvimento, sem entregar na caixa real de alguém.

## Tarefas

O Nest continua em `estante-nest`. O Next continua em `estante-next`. A chave da conta de serviço do Firebase fica fora do Git.

### Tarefa 1 — Dois avisos, dois motivos

**Faça:** no README, uma linha para o e-mail e uma para o push.

- E-mail: a pessoa pode ler depois, sem o site aberto. Exemplo: "sua conta foi criada" ou "um item seu está parado".
- Push: a pessoa está no navegador ou aceitou aviso do site. Exemplo: "a rotina encontrou itens parados".

Escolha um fato da Estante para cada um. Pode ser o mesmo fato nos dois, se você explicar por que os dois canais existem.

**Pronto quando:** os dois exemplos estão escritos e nenhum deles é "testar a ferramenta" sem um motivo do produto.

### Tarefa 2 — SMTP de estudo

**Leia:** a página do [Nodemailer](https://nodemailer.com/) até criar um transporte e enviar uma mensagem. Use a conta de teste que a documentação mostrar, ou um capturador.

**Faça:** um script solto, ainda sem fila, envia "Olá da Estante" e você abre a caixa de desenvolvimento e vê o HTML simples e o texto alternativo. Remetente, host e senha vêm de variável de ambiente.

**Pronto quando:** a mensagem está na caixa de teste e `.env` não foi commitado. `.env.example` tem as chaves vazias.

### Tarefa 3 — O envio vira job

**Faça:** o worker da sessão 21 ganha um job `enviar-email` com destinatário, assunto e texto. A API, ao criar a conta ou ao disparar o fato que você escolheu, só enfileira. Com o worker desligado, o cadastro (ou o fato) ainda conclui. Com o worker ligado, o e-mail aparece na caixa.

**Pronto quando:** você reproduz os dois estados e anota no README.

### Tarefa 4 — Falha de envio não vira conta pela metade

**Faça:** se o e-mail é consequência do cadastro, o usuário já está gravado antes do job. Se o SMTP falhar, o retry da fila tenta de novo. Não apague o usuário porque o e-mail não saiu. Escreva essa decisão.

**Pronto quando:** uma falha proposital do transporte (senha errada) deixa o usuário no banco e o job em retry ou falho, como você configurou na sessão 21.

### Tarefa 5 — Projeto Firebase

**Leia:** a visão geral de [Cloud Messaging](https://firebase.google.com/docs/cloud-messaging) e o começo de [FCM no navegador](https://firebase.google.com/docs/cloud-messaging/js/client).

**Faça:** crie um projeto no console do Firebase, adicione um app web, e baixe a configuração do cliente. A conta de serviço para o Admin SDK fica no servidor, no caminho que a [documentação do Admin](https://firebase.google.com/docs/admin/setup) indicar. O arquivo JSON da conta de serviço entra no `.gitignore`. A configuração pública do app web pode ir para variável de ambiente do Next, com o prefixo que o Next exige para o navegador, e o README lista quais campos são públicos e qual arquivo não é.

**Pronto quando:** o JSON secreto não aparece no `git status`.

### Tarefa 6 — Permissão e token

**Leia:** o trecho do guia de cliente que pede permissão e obtém o token de registro.

**Faça:** na área logada do Next, um botão "ativar avisos" pede a permissão. Se a pessoa aceitar, o token vai para `POST` autenticado no Nest, associado ao usuário. Se recusar, a tela explica que os avisos no navegador ficam desligados e o e-mail continua valendo. Não insista em loop.

**Pronto quando:** dois usuários podem ter tokens diferentes, e um usuário sem permissão continua usando a Estante.

### Tarefa 7 — O worker pede o envio ao Firebase

**Leia:** [enviar pelo servidor](https://firebase.google.com/docs/cloud-messaging/server), o caminho do Admin SDK para uma mensagem a um token.

**Faça:** um job `enviar-push` carrega os tokens daquele usuário e envia um título e um corpo curtos. Token inválido é logado e removido, se a resposta do Firebase indicar isso, seguindo o que a documentação descrever para token expirado. Não logue o token inteiro: os primeiros e os últimos caracteres bastam para você achar o registro.

**Pronto quando:** com o site aberto, ou com a permissão já dada, a notificação aparece, e o pedido HTTP que enfileirou o job não esperou o Firebase.

### Tarefa 8 — Service worker do push

**Faça:** o guia de FCM para a web pede um service worker para a mensagem quando a aba está em segundo plano. Siga esse trecho da documentação oficial. Confirme os dois casos: aba em foco e aba em segundo plano, no que o guia disser que deve acontecer em cada um.

**Pronto quando:** você descreve os dois casos com o que realmente viu. Se o navegador de estudo bloquear notificação, anote isso e use outro navegador, em vez de desligar a checagem.

### Tarefa 9 — Ligar a um fato da Estante

**Faça:** o fato da tarefa 1 enfileira e-mail, push, ou os dois, conforme a pessoa configurou. Quem não tem token não gera erro no job de e-mail. Quem não tem e-mail verificado ainda recebe o que você decidiu no escopo: se o cadastro só tem o e-mail digitado, é esse endereço, e o README diz que confirmação de caixa fica fora desta sessão.

**Pronto quando:** um fluxo completo, do fato até a caixa de teste e até a notificação, está escrito em passos no README.

### Tarefa 10 — O que não viaja no aviso

**Faça:** assunto e corpo não levam senha, hash, token de sessão nem a chave do Firebase. O push não carrega a lista inteira de itens, só uma frase e, se precisar, um id para a pessoa abrir o site.

**Pronto quando:** você revisou o payload dos dois jobs.

## Checklist

- [ ] 1. Um fato para cada canal
- [ ] 2. E-mail visível na caixa de teste
- [ ] 3. Envio só depois do worker
- [ ] 4. Falha de SMTP não desfaz o usuário
- [ ] 5. Segredo do Firebase fora do Git
- [ ] 6. Token por usuário, recusa respeitada
- [ ] 7. Push enfileirado
- [ ] 8. Primeiro plano e segundo plano descritos
- [ ] 9. Fluxo completo no README
- [ ] 10. Payload sem segredo

## Próximo passo

Siga para [25 · Deploy com Docker e Nginx](../25-deploy-docker-nginx/README.md). Redis, worker, API e site vão morar no mesmo Compose. A chave do Firebase continua fora da imagem.
