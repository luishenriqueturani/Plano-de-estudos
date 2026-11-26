# Sessão 18 — Segurança do backend

**Sessão 18 de 21 · cerca de 4 semanas**

A API passa a saber quem está chamando. Cada pessoa tem a própria estante. A senha vira hash, o acesso das rotas passa por guard, e um item só é alterado por quem é dono dele. O token JWT existe para um cliente que manda `Authorization: Bearer`. O navegador, na sessão 19, vai usar o cookie de sessão que o próprio Nest descreve, não o `localStorage`.

Esta sessão é defesa do seu programa. O critério de pronto é a API recusar o que não deve aceitar. Não procure roteiro de ataque; a documentação linkada já separa o controle do abuso.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [17 · Tarefas agendadas](../17-cron-jobs/README.md)
> **Próxima →** [19 · Segurança do frontend](../19-seguranca-frontend/README.md)

## O que você vai saber fazer

- Cadastrar e entrar com senha armazenada como hash.
- Proteger rotas com o guard de autenticação e marcar só as rotas públicas.
- Emitir access token JWT de vida curta e refresh token guardado no banco.
- Autorizar por recurso: o id na URL não basta, o dono tem que ser quem está logado.
- Ligar cabeçalhos de segurança, CORS restrito e limite de tentativas de login.

## Documentação

- [Authentication](https://docs.nestjs.com/security/authentication) — nesta sessão, só as seções citadas nas tarefas. Verificação de e-mail, segundo fator, Google e magic link ficam na mesma página para outro momento
- [Guards](https://docs.nestjs.com/guards)
- [Authorization](https://docs.nestjs.com/security/authorization)
- [Encryption and Hashing](https://docs.nestjs.com/security/encryption-and-hashing)
- [Security headers](https://docs.nestjs.com/security/helmet)
- [CORS](https://docs.nestjs.com/security/cors)
- [Rate limiting](https://docs.nestjs.com/security/rate-limiting)
- [Validação](https://docs.nestjs.com/techniques/validation)
- [JWT introduction](https://jwt.io/introduction) — o que o token carrega e o que a assinatura garante. Pare na introdução
- [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [Node.js Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Nodejs_Security_Cheat_Sheet.html)

## Tarefas

### Tarefa 1 — O que você está impedindo

**Leia:** as introduções das cheat sheets de senha, sessão e autenticação. Nome e impacto, sem procedimento.

**Faça:** no README, uma tabela com as suas palavras:

| Risco | O que acontece se você não tratar |
| --- | --- |
| Senha em texto puro | Quem copia o banco entra nas contas |
| Rota sem guard | Qualquer cliente na rede altera a coleção |
| Objeto sem dono | Trocar o id na URL mexe no item de outra pessoa |
| Campo a mais no JSON | O cliente define uma coluna que não deveria |
| Segredo no Git | O repositório entrega a chave que assina o token |
| Erro com stack na resposta | A resposta mostra caminho interno e versão |
| Origem CORS aberta com credencial | Outro site chama a API usando o navegador de quem está logado |

**Pronto quando:** a tabela está no README e cada linha tem uma tarefa desta sessão embaixo, dizendo onde você vai fechar aquele risco.

### Tarefa 2 — Usuário e dono do item

**Faça:** migration nova com a tabela de usuários que o cadastro precisar (e-mail único, hash da senha, datas). Outra coluna `usuario_id` em `itens`, obrigatória para item novo. Notas herdam o dono pelo item. Itens velhos de estudo podem ser apagados neste database: ainda não há usuário real.

**Pronto quando:** `\d` mostra as colunas e a migration tem `down`.

### Tarefa 3 — Módulo de autenticação

**Leia:** em [Authentication](https://docs.nestjs.com/security/authentication), as seções *Installation*, *Register the authentication module* e *Add sign-up and sign-in*.

**Faça:** cadastro e login da Estante seguindo essa página, com o `PasswordHasher` descrito em [Encryption and Hashing](https://docs.nestjs.com/security/encryption-and-hashing). A senha tem tamanho mínimo. O hash é o que vai para o banco. A resposta de cadastro e de login não devolve o hash.

**Pronto quando:** no `psql`, a coluna da senha não contém a senha que você digitou, e um login com senha errada não devolve o hash nem diz qual dos dois campos errou, se a documentação pedir resposta única.

### Tarefa 4 — Guard e rotas públicas

**Leia:** [Guards](https://docs.nestjs.com/guards) e, na página de autenticação, como o guard global e o decorador de rota pública funcionam na versão que você instalou.

**Faça:** rotas de itens e notas exigem usuário. Cadastro, login e uma rota `GET /saude` (sem dado) são públicas. O restante não é.

**Pronto quando:** `curl` sem credencial em `GET /itens` recebe 401, e `GET /saude` recebe 200.

### Tarefa 5 — Quem está logado

**Leia:** a seção *Read the current user*.

**Faça:** `GET /eu` devolve id e e-mail de quem apresentou a credencial, e não devolve hash nem segredo.

**Pronto quando:** dois usuários de teste recebem corpos diferentes com as próprias credenciais.

### Tarefa 6 — Access token JWT

**Leia:** *Issue tokens to the mobile app* e a [introdução do JWT](https://jwt.io/introduction).

**Faça:** configure access token de vida curta (a documentação usa 15 minutos; pode seguir). O segredo sai de variável de ambiente, com o tamanho mínimo que a página exigir. Gere com o comando que a própria página mostra (`openssl rand -base64 32`) e coloque no `.env`, nunca no Git. Issuer e audience identificam a Estante, não o exemplo `store`.

Um cliente de teste é o `curl`, com o cabeçalho `Authorization`. Decodificar o payload no [jwt.io](https://jwt.io/) serve para você **ver** os campos. Não cole o segredo nesse site: ver o payload não precisa da chave, e a chave não vai para site nenhum.

**Pronto quando:** token expirado ou adulterado recebe 401, token válido de um usuário chama `GET /eu` e volta esse usuário. O `.env.example` tem a chave com valor vazio.

### Tarefa 7 — Refresh token no banco

**Leia:** *Keep sessions and tokens in your database* e o checklist de produção, o item sobre não usar store só em memória quando `NODE_ENV=production`.

**Faça:** o refresh token da Estante persiste via migration e o store que a documentação descreve, adaptado ao TypeORM que você já tem. Renovar devolve access token novo. A tabela não guarda o token em claro se a página pedir hash.

Se o contrato do store for grande demais numa semana, implemente o pedaço que a sua configuração realmente usa (a tabela do checklist liga a feature ao contrato) e escreva o que ficou de fora. Não ligue `allowInMemoryStorage` e finja que é produção.

**Pronto quando:** reiniciar o Nest não esquece o refresh token já emitido, e um refresh token usado de um jeito que a documentação manda invalidar deixa de funcionar.

### Tarefa 8 — Autorização do dono

**Leia:** [Authorization](https://docs.nestjs.com/security/authorization), em especial escrever uma policy e checar um parâmetro de rota. A página de autenticação pede para importar o módulo de autenticação antes do de autorização.

**Faça:** listar mostra só itens do usuário atual. Criar grava `usuario_id` a partir de quem está logado, nunca do JSON. `GET`, `PATCH` e `DELETE` de um id de outra pessoa respondem 404 ou 403, a escolha que a documentação de policy usar, e não alteram a linha. Anote a escolha.

**Pronto quando:** com dois usuários, o `curl` do segundo não muda o item do primeiro. Um teste automático cobre esse caso.

### Tarefa 9 — Whitelist de novo

**Leia:** [validação](https://docs.nestjs.com/techniques/validation).

**Faça:** confirme `whitelist: true` e `forbidNonWhitelisted` se quiser erro explícito. O DTO de criar item não tem `usuarioId`. Mandar `usuarioId` no corpo não muda o dono.

**Pronto quando:** um teste envia o campo a mais e o dono gravado continua sendo quem está logado.

### Tarefa 10 — Cabeçalhos e CORS

**Leia:** [Security headers](https://docs.nestjs.com/security/helmet) e [CORS](https://docs.nestjs.com/security/cors), inclusive o aviso sobre `origin: true` junto de credencial.

**Faça:** ligue `useSecurityHeaders()` como a página mostrar. CORS aceita só a origem do Next (ainda que o Next nasça na sessão 19: use `http://localhost:3000` ou a porta que você for usar) e os métodos que a API realmente oferece. Credencial, se o cookie de sessão precisar, entra com a origem listada, não com `*`.

**Pronto quando:** `curl -I` mostra um cabeçalho que o `useSecurityHeaders` adiciona, e uma origem que não está na lista não recebe `Access-Control-Allow-Origin` liberando ela.

### Tarefa 11 — Limite no login

**Leia:** [Rate limiting](https://docs.nestjs.com/security/rate-limiting) e a seção *Rate-limit sign-in* da autenticação.

**Faça:** aplique um limite nas rotas de login e de emissão de token, mais apertado que o limite geral. Da sua máquina, passe do limite e observe o status que o Nest devolver (a documentação do throttler indica 429).

**Pronto quando:** o README registra o limite escolhido e o status observado, e as rotas de leitura normais não ficam inutilizáveis por causa do limite de login.

### Tarefa 12 — O que a resposta não carrega

**Faça:** revise os JSON de item, nota, usuário e erro. Não saem hash, segredo, stacktrace nem caminho de arquivo. O filtro da sessão 14 continua valendo para exceção não prevista.

**Pronto quando:** você procura no código as palavras `password`, `hash` e `stack` nas respostas e anota o que encontrou.

### Tarefa 13 — Checklist de produção, o que já dá

**Leia:** *Production checklist* e *Testing* na página de autenticação.

**Faça:** copie para o README só os itens que se aplicam ao que você ligou (store no banco, segredo fora do Git, HTTPS ainda não: marque como pendente da sessão 20, CSRF de cookie pendente da sessão 19 se você usa sessão por cookie). Marque feito o que já está feito, com o arquivo que prova.

**Pronto quando:** não há item "feito" sem um arquivo ou um comando correspondente.

### Tarefa 14 — Suíte mínima

**Faça:** testes automáticos para:

- senha errada não autentica;
- rota sem credencial responde 401;
- usuário A não altera item do usuário B;
- DTO não aceita dono no corpo.

**Pronto quando:** `npm test` passa e falha se você, de propósito, comentar o guard num branch que não será enviado.

## Checklist

- [ ] 1. Tabela de riscos
- [ ] 2. Migration de usuário e dono
- [ ] 3. Cadastro e login com hash
- [ ] 4. Guard e rota pública de saúde
- [ ] 5. `GET /eu`
- [ ] 6. JWT curto, segredo fora do Git
- [ ] 7. Refresh token persistido
- [ ] 8. Dono do item, com teste
- [ ] 9. `usuarioId` do corpo não manda
- [ ] 10. Cabeçalhos e CORS com origem lista
- [ ] 11. Limite no login
- [ ] 12. Resposta sem hash e sem stack
- [ ] 13. Checklist de produção honesto
- [ ] 14. Quatro testes de segurança

## Próximo passo

Siga para [19 · Segurança do frontend](../19-seguranca-frontend/README.md).
