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
| PostgreSQL | Um servidor de banco, usado da sessão 10 em diante. |
| Container | Um processo isolado, criado a partir de uma imagem. |
| Imagem | O pacote imutável com o programa que o container vai rodar. |
| Volume | O disco que sobrevive quando o container é apagado. |
| Compose | O arquivo que sobe vários containers juntos. |
| Framework | Uma base que já decide a estrutura do programa. React, Next.js e NestJS são frameworks. |
| Biblioteca | Um conjunto de funções que você chama. Quem manda no fluxo ainda é o seu código. |
| Componente | Um pedaço de interface com marcação e comportamento juntos. |
| SSR | Renderização no servidor: o HTML da página já chega montado. |
| TypeScript | JavaScript com tipos checados antes de rodar. |
| Classe | O molde: campos e métodos. Sozinha, não é um item da estante. |
| Instância | O objeto criado com `new`. Dois itens são duas instâncias da mesma classe. |
| Encapsulamento | O estado fica privado. Quem está fora muda o item por um método. |
| Composição | Um objeto que contém outros. A coleção tem itens. |
| Injeção de dependência | Receber no construtor o objeto necessário, em vez de criá-lo por dentro. Na sessão 18, quem faz isso é o Nest. |
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
| Promise | Um valor que ainda vai chegar, ou um erro. `async/await` é a forma de esperar esse valor sem aninhar callbacks. |
| Event loop | O ciclo do Node que alterna o seu código e as operações de entrada e saída. Uma thread só, muitas tarefas em andamento. |
| Concorrência | Várias tarefas em progresso ao mesmo tempo, mesmo que uma só rode de cada vez. |
| Paralelismo | Várias tarefas executando ao mesmo tempo, em threads ou processos diferentes. |
| Worker | Um processo ou thread separado que executa trabalho fora do pedido HTTP. |
| Fila | Uma lista durável de trabalhos. Quem produz o trabalho não precisa ser quem executa. |
| Tailwind | CSS por classes prontas no HTML, em vez de um arquivo de estilo escrito à mão para cada tela. |
| Métrica | Um número sobre o sistema ao longo do tempo, como quantos pedidos a API atendeu. |
| Log | O registro de um fato. Em produção, uma linha JSON com horário, nível, mensagem e id do pedido. |
| Pull request | A proposta de juntar uma mudança na branch principal, com o diff visível para outra pessoa ler. |
| Pipeline | A sequência automática de passos (teste, lint, build) que roda em cima de uma proposta. CI é esse hábito de verificar antes de juntar. |
