# Sessão 04 — JavaScript no navegador

**Sessão 4 de 27 · cerca de 8 semanas**

A página deixa de ser só documento. Você lê campos, monta fichas, guarda a coleção no navegador e organiza o código em funções. O servidor ainda não existe: se você desligar a internet, a Estante continua, porque os dados estão no `localStorage`.

Crie o repositório `estante-js`. Pode copiar o HTML e o CSS da sessão 03 como ponto de partida e registrar isso no README.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [03 · HTML e CSS](../03-html-e-css/README.md)
> **Próxima →** [05 · Ambiente Node.js](../05-ambiente-nodejs/README.md)

## O que você vai saber fazer

- Usar variáveis, condições, laços, funções, arrays e objetos com segurança.
- Alterar a página pelo DOM e reagir a eventos.
- Guardar e recuperar JSON no `localStorage`.
- Achar um erro com o depurador, em vez de espalhar `console.log` sem critério.

## Documentação

- [JavaScript dinâmico](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting)
- [Guia de JavaScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide)
- [Tipos e estruturas](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Data_structures)
- [Funções](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Functions)
- [Laços e iteração](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Loops_and_iteration)
- [Introdução ao DOM](https://developer.mozilla.org/pt-BR/docs/Web/API/Document_Object_Model/Introduction)
- [Introdução a eventos](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting/Events)
- [Manipular documentos](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting/DOM_scripting)
- [Objetos JSON](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [Web Storage](https://developer.mozilla.org/pt-BR/docs/Web/API/Web_Storage_API)
- [Depurador do Firefox](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html) ou o guia de DevTools do navegador que você usa

Os artigos do módulo Scripting incluem desafios da própria MDN. Faça os desafios do artigo no dia em que ler o artigo. As tarefas abaixo são o projeto, não um substituto dessa leitura.

## Tarefas

### Tarefa 1 — O script entra na página

**Leia:** "What is JavaScript?" no módulo [Scripting](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting).

**Faça:** `js/main.js` incluído no fim do `body`. O arquivo escreve no console o nome da Estante. Abra as ferramentas do desenvolvedor e ache essa mensagem.

**Pronto quando:** um erro de digitação no arquivo aparece no console com número de linha, e você corrige.

### Tarefa 2 — Variáveis e tipos

**Leia:** o artigo de variáveis do módulo e [tipos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Data_structures).

**Faça:** no console e depois num arquivo `js/tipos.js` (pode ser temporário), crie string, número, booleano, array e objeto representando um item da coleção: título, ano, estado. Use `const` por padrão e `let` quando o valor mudar. Não use `var`.

Compare com `===`. Anote um caso em que `==` mentiria.

**Pronto quando:** a nota existe e o objeto do item tem pelo menos quatro campos.

### Tarefa 3 — Condições

**Leia:** o artigo de condicionais do módulo Scripting.

**Faça:** uma função `rotuloEstado(estado)` que devolve um texto para `quero`, `fazendo` e `feito`, e `"desconhecido"` para qualquer outro valor.

**Pronto quando:** você testa os quatro casos no console e os resultados batem.

### Tarefa 4 — Laços

**Leia:** [laços](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Loops_and_iteration).

**Faça:** dado um array de seis itens, imprima só os que estão em `fazendo`, usando `for...of`. Conte quantos estão em `feito`.

**Pronto quando:** mudar o array muda a contagem, sem você reescrever o número na mão.

### Tarefa 5 — Funções pequenas

**Leia:** [funções](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Functions).

**Faça:** extraia três funções puras: `criarItem`, `filtrarPorEstado` e `contarPorEstado`. Pura aqui quer dizer: o resultado depende dos argumentos, e a função não mexe no DOM.

**Pronto quando:** cada função tem no máximo umas quinze linhas e você consegue dizer o que ela recebe e o que devolve.

### Tarefa 6 — Arrays: map, filter, find

**Leia:** os métodos de array no guia, a partir de [Array](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array).

**Faça:** reescreva o filtro e a contagem usando `filter` e `length`. Ache um item pelo título com `find`. Gere uma lista só de títulos com `map`.

**Pronto quando:** não sobrou laço manual nessas três operações.

### Tarefa 7 — O jogo da MDN, até o fim

**Leia e faça:** o tutorial "A first splash into JavaScript" (jogo de adivinhar o número) dentro do módulo [Scripting](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting).

Pode viver em `jogo.html`, separado da Estante.

**Pronto quando:** o jogo dá dica de maior ou menor, limita tentativas e recomeça.

### Tarefa 8 — Selecionar e criar elementos

**Leia:** [DOM scripting](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting/DOM_scripting).

**Faça:** a lista de fichas deixa de estar escrita no HTML. O HTML tem um contêiner vazio. O script cria um `article` por item do array, com título, estado e ano.

**Pronto quando:** alterar o array e recarregar muda as fichas, e o HTML inicial não contém os seis títulos.

### Tarefa 9 — Eventos

**Leia:** [eventos](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting/Events).

**Faça:** botões "todos", "quero", "fazendo" e "feito" redesenham a lista. Use uma função só de desenho, chamada por cada clique, para não copiar o laço quatro vezes.

**Pronto quando:** o botão ativo fica óbvio (classe CSS) e a lista corresponde ao filtro.

### Tarefa 10 — Formulário

**Faça:** o formulário da sessão 03 passa a ter `preventDefault`. O script lê os campos, valida título vazio e ano que não seja número, mostra a mensagem num elemento da página (não só no `alert`) e, se estiver válido, acrescenta o item na lista.

**Pronto quando:** título vazio não cria ficha, e um título válido cria, sem recarregar.

### Tarefa 11 — Apagar e editar estado

**Faça:** cada ficha tem um botão de remover e um controle para avançar o estado (`quero` → `fazendo` → `feito`). O clique chama uma função que mexe no array e desenha de novo.

**Pronto quando:** remover o terceiro item não apaga o quarto, e recarregar a página ainda perde os dados. A perda é proposital até a tarefa 13. Escreva isso no README para você não "corrigir" antes da hora.

### Tarefa 12 — JSON

**Leia:** [JSON](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/JSON).

**Faça:** `JSON.stringify` no array e `JSON.parse` de volta. Provoque um erro de parse com texto inválido e capture com `try/catch`, mostrando uma mensagem sua.

**Pronto quando:** você explica a diferença entre o objeto e o texto JSON.

### Tarefa 13 — localStorage

**Leia:** [Web Storage](https://developer.mozilla.org/pt-BR/docs/Web/API/Web_Storage_API).

**Faça:** grave a coleção a cada inclusão, remoção ou troca de estado. Ao abrir a página, leia a chave. Se não houver nada, comece com três itens de exemplo.

**Pronto quando:** fechar o navegador e abrir de novo mantém a coleção. Limpar o armazenamento do site zera, e a página não quebra.

### Tarefa 14 — Busca

**Faça:** um campo de texto filtra pelo título, combinado com o filtro de estado. Os dois funcionam juntos.

**Pronto quando:** buscar "a" dentro de "fazendo" não mostra itens "quero" que também tenham "a".

### Tarefa 15 — Escopo e um bug de propósito

**Leia:** a seção de escopo no guia de funções.

**Faça:** escreva no README um bug que você realmente encontrou nesta sessão: o que aparecia, a causa e o conserto. Se ainda não encontrou nenhum, introduza uma variável global que duas funções disputam, observe o efeito e corrija passando o valor por argumento.

**Pronto quando:** o README tem essa história, curta e específica.

### Tarefa 16 — Depurador

**Faça:** coloque um breakpoint na função que desenha a lista. Recarregue, clique num filtro e inspecione o array no painel de escopo. Anote como você fez, no navegador que usa.

**Pronto quando:** você descreve o valor de uma variável parado no meio da função, visto no depurador e não no console.

### Tarefa 17 — Acessibilidade dos controles novos

**Faça:** botões são `button`, não `div` com clique. O campo de busca tem `label`. Depois de incluir um item, o foco vai para um lugar sensato (o campo de título, já limpo, ou a nova ficha). Ícone sem texto tem nome acessível.

**Pronto quando:** dá para incluir e apagar um item só com teclado.

### Tarefa 18 — Organizar arquivos

**Faça:** separe, se o projeto ainda estiver num arquivo só:

- `estado.js` — ler e gravar a coleção
- `render.js` — desenhar
- `main.js` — ligar eventos

Sem empacotador. Vários `script` no HTML, na ordem certa, bastam. Módulos com `type="module"` são bem-vindos se você ler antes [módulos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Modules). Escolha um dos dois jeitos e explique no README.

**Pronto quando:** nenhuma função de desenho grava no `localStorage`, e nenhuma função de armazenamento mexe no DOM.

### Tarefa 19 — Página de detalhe a partir do id

**Faça:** cada item ganha um `id` (número ou texto único). O link da ficha abre `item.html?id=...`. Essa página lê a coleção e mostra o item, ou uma mensagem clara se o id não existir.

**Pronto quando:** um id inventado na barra de endereço não gera página em branco nem erro sem texto.

### Tarefa 20 — Revisão

**Faça:**

- Apague `console.log` que não servem mais.
- Releia as funções com mais de vinte linhas e quebre uma delas.
- Atualize o README: como abrir (basta o arquivo ou precisa de um servidor estático por causa dos módulos), o que fica salvo, o que se perde se limpar o site.
- Publique de novo, se quiser, com GitHub Pages.

**Pronto quando:** uma pessoa que clona o repositório chega na lista em poucos minutos, seguindo só o README.

## Checklist

- [ ] 1. Script carregado e visível no console
- [ ] 2. Tipos e `===`
- [ ] 3. `rotuloEstado`
- [ ] 4. Laço sobre a coleção
- [ ] 5. Três funções puras
- [ ] 6. `map`, `filter` e `find`
- [ ] 7. Jogo da MDN
- [ ] 8. Fichas criadas pelo script
- [ ] 9. Filtros por estado
- [ ] 10. Formulário com validação visível
- [ ] 11. Remover e avançar estado
- [ ] 12. JSON com erro tratado
- [ ] 13. `localStorage`
- [ ] 14. Busca combinada com o filtro
- [ ] 15. Um bug explicado
- [ ] 16. Breakpoint usado
- [ ] 17. Teclado suficiente para incluir e apagar
- [ ] 18. Arquivos separados por responsabilidade
- [ ] 19. Detalhe por `id`
- [ ] 20. README que coloca o projeto de pé

## Próximo passo

Siga para [05 · Ambiente Node.js e nvm](../05-ambiente-nodejs/README.md).
