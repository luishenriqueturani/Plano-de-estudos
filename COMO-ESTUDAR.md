# Como estudar

[Índice](README.md) · [Progresso](PROGRESSO.md) · [Glossário](GLOSSARIO.md)

Este repositório é o mapa. Os projetos ficam em repositórios separados, na sua conta do GitHub. Assim o histórico de cada fase continua legível.

## Ritmo de uma semana

Reserve de 6 a 8 horas, em quatro ou cinco encontros. Um encontro curto todo dia rende mais do que um sábado inteiro a cada quinze dias.

Em cada encontro:

1. Abra a sessão atual e escolha a primeira tarefa ainda desmarcada.
2. Leia só os links daquela tarefa. Anote três frases com as suas palavras no README do projeto.
3. Faça a entrega pedida.
4. Rode de novo e confira o critério **Pronto quando**.
5. Faça um commit. A mensagem diz o que passou a existir, por exemplo: `adiciona listagem de itens no sqlite`.
6. Marque a tarefa no checklist do fim da sessão.

No fim da semana, releia o que commitou e escreva cinco linhas: o que funcionou, o que travou, o que ficou confuso.

## Quando travar

Fique 40 minutos no problema antes de pedir ajuda. Nesse tempo:

- leia de novo o trecho da documentação linkado, não um tutorial aleatório;
- reduza o caso: o menor arquivo que ainda mostra o erro;
- copie a mensagem de erro inteira e o comando que você rodou.

Aí sim pergunte, colando o que tentou. Pergunta com erro, comando e o que você esperava que acontecesse recebe uma resposta útil. "Não funciona" não recebe.

Se uma tarefa opcional consumir mais de dois encontros, deixe-a marcada como adiada e siga. Tarefa sem a palavra "opcional" faz parte do caminho.

## Onde guardar as coisas

No seu computador, uma pasta só para código:

```text
~/projetos/
  plano-de-estudos/     este mapa, se você clonou para marcar o progresso
  estante-html/
  estante-js/
  estante-cli/
  estante-api/
  estante-react/
  estante-next/
  estante-nest/
  estante-final/
```

A sessão de Git explica como criar esses repositórios. Cada um começa copiando o [modelo de README](modelo-readme-atividade.md).

## O que escrever no README de cada projeto

- o que o programa faz;
- como instalar e rodar, com os comandos reais;
- o que você entendeu, em poucas frases;
- o que ficou de fora de propósito.

Quem abre o GitHub do projeto, inclusive você daqui a três meses, precisa conseguir rodar sem adivinhar.

## Como marcar progresso

Os checkboxes deste repositório não mudam sozinhos no site do GitHub. Edite o arquivo, marque `[x]` e faça commit. Há duas listas:

- o checklist no final de cada sessão, tarefa por tarefa;
- o [Progresso](PROGRESSO.md), uma linha por sessão.

## Fontes

A documentação linkada em cada tarefa é a referência. Quando um site de curso discordar da documentação, fique com a documentação.

No CachyOS, pacote, serviço e permissão de sistema seguem a [wiki do Arch](https://wiki.archlinux.org/), porque o CachyOS é baseado no Arch. A [wiki do CachyOS](https://wiki.cachyos.org/) entra quando o assunto for específico da distribuição.

Não copie um comando com `sudo` de um comentário de fórum. Se a tarefa pedir um comando de sistema, ele aponta para a wiki ou para o guia oficial.

## O que conta como "aprendi"

Você consegue explicar em voz alta, sem ler a tela, o que aquela parte faz. E consegue refazer a parte central sem colar o arquivo antigo do lado. Refazer uma tela ou uma rota no dia seguinte é o melhor teste que existe nesta trilha.
