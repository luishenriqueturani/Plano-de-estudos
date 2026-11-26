# Plano de estudos de desenvolvimento web

Um caminho contínuo para quem está começando a programar. A trilha sai do terminal, passa por páginas, JavaScript, bancos, React e Next.js, e chega numa aplicação com API em NestJS, PostgreSQL, autenticação e publicação com Docker e Nginx.

O ritmo sugerido é de **6 a 8 horas por semana**, o que leva cerca de **18 meses**. Dá para ir mais rápido. O volume foi pensado para passar de um ano mesmo assim: cada sessão tem leitura na documentação oficial e um lote de tarefas com entrega concreta.

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

### Fase 2 — Servidor e dados (cerca de 20 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 05 | [Ambiente Node.js e nvm](sessoes/05-ambiente-nodejs/README.md) | 1 semana | Node LTS instalado pelo nvm |
| 06 | [Node.js e lógica de programação](sessoes/06-nodejs-e-logica/README.md) | 6 semanas | CLI que grava a coleção em arquivo |
| 07 | [Banco de dados com SQLite](sessoes/07-sqlite/README.md) | 3 semanas | A mesma CLI usando SQL |
| 08 | [Docker e PostgreSQL](sessoes/08-docker-e-postgresql/README.md) | 3 semanas | Postgres no Docker, com os dados da Estante |
| 09 | [Cliente e servidor](sessoes/09-cliente-servidor/README.md) | 2 semanas | API HTTP mínima e o contrato das rotas |
| 10 | [Frontend e backend juntos](sessoes/10-fullstack-html-css-js-node/README.md) | 5 semanas | Página que cria, lista, edita e apaga no Postgres |

### Fase 3 — Interface e tipos (cerca de 16 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 11 | [React com Vite](sessoes/11-react-com-vite/README.md) | 6 semanas | A Estante reescrita em componentes |
| 12 | [TypeScript com Node](sessoes/12-typescript-com-node/README.md) | 4 semanas | Um módulo do servidor tipado |
| 13 | [Next.js, TypeScript e SSR](sessoes/13-nextjs/README.md) | 6 semanas | Projeto fullstack em Next.js |

### Fase 4 — API profissional (cerca de 18 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 14 | [NestJS](sessoes/14-nestjs/README.md) | 5 semanas | API organizada em módulos |
| 15 | [TypeORM](sessoes/15-typeorm/README.md) | 3 semanas | CRUD com entidades e relações |
| 16 | [Migrations e seeders](sessoes/16-migrations-e-seeders/README.md) | 2 semanas | Banco criado por migration e seed |
| 17 | [Tarefas agendadas](sessoes/17-cron-jobs/README.md) | 1 semana | Um cron job com função real |
| 18 | [Segurança do backend](sessoes/18-seguranca-backend/README.md) | 4 semanas | Login, JWT, guards e checagens de acesso |
| 19 | [Segurança do frontend](sessoes/19-seguranca-frontend/README.md) | 3 semanas | Next.js autenticado falando com o Nest |

### Fase 5 — Publicar e fechar (cerca de 9 semanas)

| | Sessão | Duração | Você entrega |
| --- | --- | --- | --- |
| 20 | [Deploy com Docker e Nginx](sessoes/20-deploy-docker-nginx/README.md) | 3 semanas | Stack inteira atrás do Nginx |
| 21 | [Projeto final](sessoes/21-projeto-final/README.md) | 6 semanas | Aplicação sua, do banco até a tela |

## O que este plano não é

Não é uma lista de cursos para assistir até o fim. A documentação oficial é a fonte. Vídeo ajuda quando um conceito não entra, e a tarefa continua sendo o que prova que você aprendeu.

Não pule uma sessão porque o nome da próxima parece mais interessante. Next.js e NestJS usam tudo o que vem antes: HTTP, SQL, JavaScript e tipos.

## Começar

Abra [Como estudar](COMO-ESTUDAR.md) e, em seguida, a [sessão 01](sessoes/01-terminal-linux/README.md).
