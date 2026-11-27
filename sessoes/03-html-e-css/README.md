# Sessão 03 — HTML e CSS

**Sessão 3 de 27 · cerca de 5 semanas**

A Estante vira um site de verdade: várias páginas, layout que cabe no celular e publicação no GitHub Pages. Ainda não há JavaScript. Se a página só funciona com script, ela ficou para a próxima sessão.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [02 · Git e GitHub](../02-git-e-github/README.md)
> **Próxima →** [04 · JavaScript no navegador](../04-javascript-no-navegador/README.md)

## O que você vai saber fazer

- Escrever HTML semântico: o nome da tag diz o que aquilo é.
- Estilizar com CSS externo, flexbox e grid.
- Fazer a página funcionar em tela estreita.
- Publicar o site num endereço público.

## Documentação

- [Aprender desenvolvimento web](https://developer.mozilla.org/pt-BR/docs/Learn_web_development)
- [Estruturar conteúdo com HTML](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Structuring_content)
- [Referência de HTML](https://developer.mozilla.org/pt-BR/docs/Web/HTML)
- [Elementos de seção](https://developer.mozilla.org/pt-BR/docs/Web/HTML/Reference/Elements/Heading_Elements)
- [Formulários](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Forms)
- [Fundamentos de CSS](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Styling_basics)
- [Referência de CSS](https://developer.mozilla.org/pt-BR/docs/Web/CSS)
- [Modelo de caixas](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Styling_basics/Box_model)
- [Flexbox](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Guides/Flexible_box_layout)
- [Grid](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Guides/Grid_layout)
- [Media queries](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Guides/Media_queries)
- [GitHub Pages](https://docs.github.com/pt/pages/getting-started-with-github-pages/creating-a-github-pages-site)

Se algum link em inglês abrir, a versão em português costuma estar no seletor de idioma da própria MDN. O caminho do artigo é o mesmo.

## Tarefas

Trabalhe em `~/projetos/estante-html`. Um commit por tarefa.

### Tarefa 1 — Página mínima válida

**Leia:** o primeiro artigo de [Structuring content](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Structuring_content) (sintaxe e estrutura do documento).

**Faça:** `index.html` com `doctype`, `html` e `lang="pt-BR"`, `head` (`meta charset`, `meta viewport`, `title`) e `body` com um `h1`.

Abra o arquivo no navegador pelo caminho local.

**Pronto quando:** o título da aba é o nome da sua Estante e a página declara português.

### Tarefa 2 — Texto com hierarquia

**Leia:** o artigo de títulos e parágrafos do mesmo módulo.

**Faça:** uma apresentação curta: o que é a coleção, de quem é, como os itens são escolhidos. Use um único `h1`, depois `h2`, parágrafos e uma lista.

**Pronto quando:** você não usa tag de título só para deixar a letra grande. Tamanho fica para o CSS.

### Tarefa 3 — Links internos e externos

**Leia:** o artigo de links do módulo de HTML.

**Faça:**

- `sobre.html` com um parágrafo sobre você.
- Links de ida e volta entre `index.html` e `sobre.html`.
- Um link externo que abre a documentação que você está usando, com texto que diz para onde vai. "Clique aqui" não serve.

**Pronto quando:** os três links funcionam com o arquivo aberto pelo navegador, sem servidor.

### Tarefa 4 — Imagens com texto alternativo

**Leia:** o artigo de imagens do módulo de HTML.

**Faça:** uma imagem de capa (pode ser um desenho simples exportado ou uma foto sua, com direito de uso). O atributo `alt` descreve a imagem para quem não a vê. Imagem decorativa leva `alt=""`.

**Pronto quando:** você consegue justificar o texto do `alt` em uma frase.

### Tarefa 5 — Regiões da página

**Leia:** a parte de estrutura semântica do módulo (header, nav, main, footer, section, article).

**Faça:** as duas páginas passam a ter cabeçalho com navegação, conteúdo principal e rodapé. O rodapé leva o seu nome e o ano.

**Pronto quando:** um leitor de estrutura (a árvore de acessibilidade do navegador, no DevTools) mostra essas regiões. Procure "Accessibility" nas ferramentas do desenvolvedor.

### Tarefa 6 — Seis itens estáticos

**Faça:** em `index.html`, seis fichas da coleção. Cada ficha é um `article` com título, uma linha de descrição, o ano ou a data e um estado (`quero`, `fazendo`, `feito`). Ainda é HTML puro, repetido na mão.

**Pronto quando:** as seis fichas estão no ar localmente e um commit registra isso.

### Tarefa 7 — Página de detalhe

**Faça:** `item.html` para um dos itens, com texto mais longo, uma lista do que você achou relevante e o link de volta. Não precisa ser dinâmico: uma página já basta para treinar o conteúdo.

**Pronto quando:** dá para ir da lista até o detalhe e voltar.

### Tarefa 8 — Formulário que ainda não envia

**Leia:** [Forms](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Forms), a introdução e os campos básicos.

**Faça:** `sugerir.html` com formulário: título (texto obrigatório), tipo (select), estado (rádio), comentário (textarea) e botão. Use `label` ligado ao `id` de cada campo. O `action` pode ficar vazio. Na sessão 04 o JavaScript passa a ler esses campos.

**Pronto quando:** clicar no texto do rótulo foca o campo, e o título vazio não passa pela validação nativa do navegador.

### Tarefa 9 — CSS externo e o modelo de caixas

**Leia:** [What is CSS?](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Styling_basics) e o [modelo de caixas](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Styling_basics/Box_model).

**Faça:**

- `css/estilos.css` linkado por todas as páginas.
- Defina uma fonte, a cor do texto, a cor do fundo e uma largura máxima para o conteúdo.
- Dê margem e padding numa ficha e desenhe, nas notas, o que é conteúdo, padding, borda e margem.

**Pronto quando:** nenhuma página tem `style=""` espalhado nas tags.

### Tarefa 10 — Seletores

**Leia:** o artigo de seletores do módulo de CSS.

**Faça:** estilize `article` de um jeito, o link do estado atual da navegação de outro, e o botão do formulário de outro. Use classe quando o elemento sozinho for genérico demais. Evite `id` no CSS.

**Pronto quando:** a navegação mostra em qual página a pessoa está, só com classe no HTML e regra no CSS.

### Tarefa 11 — Variáveis e tipografia

**Leia:** [variáveis CSS](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Reference/Values/var) e o módulo de texto da MDN, se quiser ir além ([CSS text styling](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Text_styling), a partir do índice [Core](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core)).

**Faça:** cores e um espaçamento base em variáveis no `:root`. Ajuste `line-height` do texto corrido. Títulos não usam mais do que três tamanhos.

**Pronto quando:** mudar a cor principal exige editar um único lugar.

### Tarefa 12 — Flexbox no cabeçalho e nas fichas

**Leia:** [Flexbox](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Guides/Flexible_box_layout).

**Faça:** a navegação fica em linha, com espaço entre o nome do site e os links. As fichas se alinham numa fileira que quebra quando falta espaço (`flex-wrap`).

**Pronto quando:** estreitar a janela faz as fichas descerem, sem barra de rolagem horizontal.

### Tarefa 13 — Grid na lista

**Leia:** [Grid](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Guides/Grid_layout).

**Faça:** a lista de fichas passa a ser grid. Em tela larga, três colunas. Você pode manter o flexbox no cabeçalho.

**Pronto quando:** as colunas têm a mesma largura e o vão entre elas vem da propriedade `gap`, não de margem negativa.

### Tarefa 14 — Mobile primeiro

**Leia:** [media queries](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Guides/Media_queries).

**Faça:** o CSS base é a tela estreita (uma coluna, navegação empilhada ou um bloco simples). A partir de uma largura que você escolher, entram as três colunas. Teste no modo responsivo do navegador em 360px e em 1200px.

**Pronto quando:** os dois tamanhos estão apresentáveis e o texto não encosta na borda da tela.

### Tarefa 15 — Estados de interação

**Leia:** a parte de hover, focus e a pseudo-classe `:focus-visible` na referência de CSS.

**Faça:** links e o botão têm estado visível ao teclado. Não remova o contorno sem colocar outro indicador. `:hover` sozinho não basta, porque teclado não faz hover.

**Pronto quando:** você percorre a página inteira com Tab e sempre sabe onde está o foco.

### Tarefa 16 — Contraste e linguagem

**Leia:** o básico de [acessibilidade na MDN](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Accessibility) (o módulo introdutório; se o caminho mudar, busque "Accessibility" dentro de Learn web development).

**Faça:**

- Confira contraste do texto com uma ferramenta do navegador ou com o que a MDN indicar.
- O `title` de cada página é específico ("Estante · Sobre", não só "Sobre").
- Não há informação só pela cor: o estado do item também está escrito.

**Pronto quando:** uma pessoa que não distingue as cores ainda lê o estado do item.

### Tarefa 17 — Revisão do HTML no validador

**Faça:** cole as páginas no [validador da W3C](https://validator.w3.org/) (opção de entrada direta). Corrija erros. Avisos merecem leitura; erro, correção.

**Pronto quando:** as páginas principais não têm erro de validação.

### Tarefa 18 — Publicar no GitHub Pages

**Leia:** [Creating a GitHub Pages site](https://docs.github.com/pt/pages/getting-started-with-github-pages/creating-a-github-pages-site).

**Faça:** ative Pages a partir da branch `main`, pasta raiz (ou `/docs`, se você preferir mover o site para lá e ajustar os caminhos). Espere o endereço ficar no ar. Coloque a URL no README.

Caminhos de CSS e de página devem funcionar no subendereço do Pages. Se o CSS sumir, o caminho estava absoluto demais: use caminhos relativos.

**Pronto quando:** outra pessoa abre a URL e vê as seis fichas, sem clonar o repositório.

### Tarefa 19 — Opcional: uma página só de um autor ou gênero

**Faça:** filtre visualmente, ainda na mão, uma segunda listagem (`quero.html` ou o critério que fizer sentido na sua coleção). O cabeçalho continua o mesmo. Se você copiou e colou o cabeçalho, anote isso no README: na sessão 04 e, de novo, no React, essa repetição deixa de ser manual.

**Pronto quando:** a página nova também está no Pages.

## Checklist

- [ ] 1. Documento válido com idioma
- [ ] 2. Hierarquia de títulos
- [ ] 3. Links com texto claro
- [ ] 4. Imagem com `alt`
- [ ] 5. Header, nav, main, footer
- [ ] 6. Seis fichas
- [ ] 7. Página de detalhe
- [ ] 8. Formulário com rótulos
- [ ] 9. CSS externo e caixas anotadas
- [ ] 10. Seletores sem `style` inline
- [ ] 11. Variáveis de cor
- [ ] 12. Flexbox
- [ ] 13. Grid
- [ ] 14. Mobile primeiro, testado em dois tamanhos
- [ ] 15. Foco visível no teclado
- [ ] 16. Estado não depende só da cor
- [ ] 17. Validador sem erros
- [ ] 18. URL pública no README
- [ ] 19. Opcional: segunda listagem

## Próximo passo

Siga para [04 · JavaScript no navegador](../04-javascript-no-navegador/README.md).
