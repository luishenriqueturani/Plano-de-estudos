# Glossário

[Índice](README.md) · [Como estudar](COMO-ESTUDAR.md)

Definições curtas para destravar a leitura. O detalhe mora na documentação linkada em cada sessão.

| Termo | Significado neste plano |
| --- | --- |
| HTML | A estrutura da página: títulos, parágrafos, links, formulários. |
| CSS | A apresentação: cor, espaço, tipo de letra, layout. |
| JavaScript | A linguagem que muda a página e, no Node, também roda no servidor. |
| DOM | A árvore da página que o JavaScript lê e altera no navegador. |
| Terminal | O programa de texto onde você roda comandos. |
| Repositório | A pasta do projeto com o histórico de commits. |
| Commit | Um registro nomeado de uma mudança. |
| Branch | Uma linha de histórico. Você usa para experimentar sem bagunçar a principal. |
| Node.js | O programa que executa JavaScript fora do navegador. |
| nvm | O gerenciador que instala e troca versões do Node. |
| npm | O gerenciador de pacotes que vem com o Node. |
| Pacote | Código de terceiros que você instala, ou o seu próprio projeto descrito no `package.json`. |
| JSON | Texto estruturado em chaves e listas. É o formato mais comum entre cliente e servidor neste plano. |
| API | Um conjunto de operações que um programa oferece a outro. Aqui, rotas HTTP. |
| HTTP | O protocolo da web. Cada chamada tem método, caminho, cabeçalhos, corpo e um código de status. |
| Cliente | Quem pede. No navegador, a página. |
| Servidor | Quem responde. No plano, um processo Node escutando uma porta. |
| Rota | A combinação de método e caminho, como `GET /itens`. |
| SQL | A linguagem para criar tabelas e consultar dados relacionais. |
| Tabela | Um conjunto de linhas com as mesmas colunas. |
| Chave primária | O identificador único de uma linha. |
| Chave estrangeira | Uma coluna que aponta para a chave primária de outra tabela. |
| SQLite | Um banco inteiro dentro de um arquivo. Bom para aprender SQL. |
| PostgreSQL | Um servidor de banco, usado da sessão 08 em diante. |
| Container | Um processo isolado, criado a partir de uma imagem. |
| Imagem | O pacote imutável com o programa que o container vai rodar. |
| Volume | O disco que sobrevive quando o container é apagado. |
| Compose | O arquivo que sobe vários containers juntos. |
| Framework | Uma base que já decide a estrutura do programa. React, Next.js e NestJS são frameworks. |
| Biblioteca | Um conjunto de funções que você chama. Quem manda no fluxo ainda é o seu código. |
| Componente | Um pedaço de interface com marcação e comportamento juntos. |
| SSR | Renderização no servidor: o HTML da página já chega montado. |
| TypeScript | JavaScript com tipos checados antes de rodar. |
| DTO | O formato combinado de entrada ou saída de uma rota. |
| ORM | Uma camada que representa tabelas como classes. Neste plano, o TypeORM. |
| Entidade | A classe que o TypeORM associa a uma tabela. |
| Migration | Um arquivo versionado que altera o esquema do banco. |
| Seed | Dados iniciais e repetíveis, como a lista de categorias. |
| Cron | Um horário recorrente para executar uma função. |
| Hash de senha | A transformação irreversível da senha antes de gravar. |
| JWT | Um token assinado que o servidor confere sem guardar a sessão em memória. |
| Guard | A classe do Nest que decide se a rota pode executar. |
| Cookie | Um dado pequeno que o navegador reenvia ao servidor. Com `HttpOnly`, o JavaScript da página não o lê. |
| CORS | A regra do navegador sobre qual outro site pode chamar a sua API. |
| Proxy reverso | Um servidor na frente dos outros. Aqui, o Nginx recebe o pedido e encaminha para o Next ou para o Nest. |
| Variável de ambiente | Configuração que fica fora do código, como porta, usuário do banco e segredo do token. |
