# Plano de estudos de desenvolvimento web

Um caminho contínuo para quem está começando a programar. A trilha sai do terminal, passa por páginas, JavaScript, bancos, React, Next.js e uma API em NestJS, e chega em autenticação, filas, avisos, observabilidade e publicação com Docker e Nginx.

O ritmo sugerido é de **6 a 8 horas por semana**, o que leva cerca de **22 meses**. Dá para ir mais rápido. O volume foi pensado para passar de um ano mesmo assim: cada sessão tem leitura na documentação oficial e um lote de tarefas com entrega concreta.

O projeto que atravessa o plano se chama **Estante**: uma coleção pessoal (livros, jogos, links de estudo ou o tema que você escolher). Ele começa como página estática e termina como aplicação completa. Troque o tema se quiser. Mantenha o mesmo domínio do começo ao fim, para comparar o que cada ferramenta muda.

## Como navegar

1. Leia [Como estudar](COMO-ESTUDAR.md).
2. Abra a sessão 01 e faça as tarefas na ordem.
3. No fim de cada sessão, o link **Próxima** leva à seguinte.
4. Marque a sessão em [Progresso](PROGRESSO.md) quando o checklist dela estiver completo.
5. Publique cada entrega num repositório seu. O modelo de README está em [modelo de README da atividade](modelo-readme-atividade.md).

Termos que vão aparecer no caminho estão no [Glossário](GLOSSARIO.md).

## Mapa da trilha

### Fase 1 — Base (cerca de 16 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 01 | [Terminal no Linux (CachyOS)](sessoes/01-terminal-linux/README.md) | 1 semana | Pastas organizadas e comandos anotados |
| 02 | [Git e GitHub](sessoes/02-git-e-github/README.md) | 2 semanas | Conta, chave e repositório da Estante |
| 03 | [HTML e CSS](sessoes/03-html-e-css/README.md) | 5 semanas | Site estático publicado |
| 04 | [JavaScript no navegador](sessoes/04-javascript-no-navegador/README.md) | 8 semanas | Estante interativa, ainda sem servidor |

### Fase 2 — Servidor e dados (cerca de 25 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 05 | [Ambiente Node.js e nvm](sessoes/05-ambiente-nodejs/README.md) | 1 semana | Node LTS instalado pelo nvm |
| 06 | [Node.js e lógica de programação](sessoes/06-nodejs-e-logica/README.md) | 6 semanas | CLI que grava a coleção em arquivo |
| 07 | [Async e await](sessoes/07-async-e-await/README.md) | 3 semanas | A CLI espera o disco sem callback aninhado |
| 08 | [Concorrência e paralelismo](sessoes/08-concorrencia-e-paralelismo/README.md) | 2 semanas | Event loop e um worker thread, com a diferença escrita |
| 09 | [Banco de dados com SQLite](sessoes/09-sqlite/README.md) | 3 semanas | A mesma CLI usando SQL |
| 10 | [Docker e PostgreSQL](sessoes/10-docker-e-postgresql/README.md) | 3 semanas | Postgres no Docker, com os dados da Estante |
| 11 | [Cliente e servidor](sessoes/11-cliente-servidor/README.md) | 2 semanas | API HTTP mínima e o contrato das rotas |
| 12 | [Frontend e backend juntos](sessoes/12-fullstack-html-css-js-node/README.md) | 5 semanas | Página que cria, lista, edita e apaga no Postgres |

### Fase 3 — Interface e tipos (cerca de 18 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 13 | [React com Vite](sessoes/13-react-com-vite/README.md) | 6 semanas | A Estante reescrita em componentes |
| 14 | [Tailwind](sessoes/14-tailwind/README.md) | 2 semanas | A mesma interface com classes utilitárias |
| 15 | [TypeScript com Node](sessoes/15-typescript-com-node/README.md) | 4 semanas | Um módulo do servidor tipado |
| 16 | [Next.js, TypeScript e SSR](sessoes/16-nextjs/README.md) | 6 semanas | Projeto fullstack em Next.js |

### Fase 4 — API profissional (cerca de 23 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 17 | [NestJS](sessoes/17-nestjs/README.md) | 5 semanas | API organizada em módulos |
| 18 | [TypeORM](sessoes/18-typeorm/README.md) | 3 semanas | CRUD com entidades e relações |
| 19 | [Migrations e seeders](sessoes/19-migrations-e-seeders/README.md) | 2 semanas | Banco criado por migration e seed |
| 20 | [Tarefas agendadas](sessoes/20-cron-jobs/README.md) | 1 semana | Um cron job com função real |
| 21 | [Workers, filas e Kafka](sessoes/21-workers-filas-e-kafka/README.md) | 3 semanas | Um worker com fila e um evento no Kafka |
| 22 | [Segurança do backend](sessoes/22-seguranca-backend/README.md) | 4 semanas | Login, JWT, guards e checagens de acesso |
| 23 | [Segurança do frontend](sessoes/23-seguranca-frontend/README.md) | 3 semanas | Next.js autenticado falando com o Nest |
| 24 | [E-mail e push](sessoes/24-email-e-push/README.md) | 2 semanas | Um e-mail e uma notificação push de verdade |

### Fase 5 — Publicar e fechar (cerca de 12 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 25 | [Deploy com Docker e Nginx](sessoes/25-deploy-docker-nginx/README.md) | 3 semanas | Stack inteira atrás do Nginx |
| 26 | [Observabilidade](sessoes/26-observabilidade/README.md) | 3 semanas | Métrica no Grafana e log JSON com id do pedido |
| 27 | [Projeto final](sessoes/27-projeto-final/README.md) | 6 semanas | Aplicação sua, do banco até a tela |

## O que este plano não é

Não é uma lista de cursos para assistir até o fim. A documentação oficial é a fonte. Vídeo ajuda quando um conceito não entra, e a tarefa continua sendo o que prova que você aprendeu.

Não pule uma sessão porque o nome da próxima parece mais interessante. Next.js e NestJS usam tudo o que vem antes: HTTP, SQL, JavaScript e tipos.

## Começar

Abra [Como estudar](COMO-ESTUDAR.md) e, em seguida, a [sessão 01](sessoes/01-terminal-linux/README.md).
