# Sessão 02 — Git e GitHub

**Sessão 2 de 21 · cerca de 2 semanas**

Cada atividade deste plano mora num repositório. Esta sessão cria a conta, o hábito de commit pequeno e o primeiro repositório da Estante, ainda sem página.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [01 · Terminal](../01-terminal-linux/README.md)
> **Próxima →** [03 · HTML e CSS](../03-html-e-css/README.md)

## O que você vai saber fazer

- Gravar o histórico de uma pasta com Git.
- Ler o que mudou antes de commitar.
- Enviar o repositório para o GitHub por SSH.
- Clonar, alterar e enviar de novo.
- Abrir uma issue para não perder a próxima tarefa.

## Documentação

- [Pro Git, em português](https://git-scm.com/book/pt-br/v2) — capítulos 1 e 2 agora; o 3 entra na tarefa de branches
- [Referência do Git](https://git-scm.com/docs)
- [Começar no GitHub](https://docs.github.com/pt/get-started)
- [Hello World do GitHub](https://docs.github.com/pt/get-started/start-your-journey/hello-world)
- [Conectar com SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh)
- [Sobre READMEs](https://docs.github.com/pt/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)
- [Git — wiki do Arch](https://wiki.archlinux.org/title/Git)

O link de README acima segue a documentação atual. Se o GitHub mover a página, busque "About READMEs" dentro de [docs.github.com/pt](https://docs.github.com/pt).

## Tarefas

### Tarefa 1 — Dizer o que o Git guarda

**Leia:** [Pro Git, sobre controle de versão](https://git-scm.com/book/pt-br/v2/Primeiros-Passos-Sobre-Controle-de-Versão) e [o que é o Git](https://git-scm.com/book/pt-br/v2/Primeiros-Passos-O-que-é-Git%3F).

**Faça:** no arquivo `~/projetos/notas/git.md`, explique com as suas palavras: repositório, commit, área de stage e working tree.

**Pronto quando:** as quatro explicações existem e não são uma cópia do livro.

### Tarefa 2 — Identidade local

**Leia:** [Pro Git, configuração inicial](https://git-scm.com/book/pt-br/v2/Primeiros-Passos-Configuração-inicial-do-Git).

**Faça:**

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email-do-github"
git config --global init.defaultBranch main
git config --global --list
```

Use o mesmo e-mail da conta que você vai criar no GitHub.

**Pronto quando:** `git config --global --list` mostra nome, e-mail e `main`.

### Tarefa 3 — Primeiro repositório, só na sua máquina

**Leia:** [Pro Git, gravando alterações](https://git-scm.com/book/pt-br/v2/Fundamentos-do-Git-Gravando-Alterações-no-Repositório).

**Faça:**

```bash
mkdir ~/projetos/treino-git
cd ~/projetos/treino-git
git init
```

- Crie `notas.md` com três linhas.
- Rode `git status` antes e depois de `git add`.
- Faça o commit.

**Pronto quando:** `git log` mostra um commit e `git status` diz que a árvore está limpa.

### Tarefa 4 — Ver a mudança antes de gravar

**Faça:**

- Altere `notas.md`.
- Rode `git diff` antes do `git add`, e `git diff --staged` depois.
- Faça um segundo commit.
- Rode `git show` no último commit.

**Pronto quando:** você aponta, na saída do `diff`, a linha que entrou e a linha que saiu.

### Tarefa 5 — Ignorar o que não é do projeto

**Leia:** a seção de gitignore em [gravando alterações no Pro Git](https://git-scm.com/book/pt-br/v2/Fundamentos-do-Git-Gravando-Alterações-no-Repositório).

**Faça:**

- Crie `.gitignore` com `rascunho.txt` e a pasta `tmp/`.
- Crie esses arquivos e rode `git status`.
- Commite só o `.gitignore`.

**Pronto quando:** `git status` não lista `rascunho.txt` nem `tmp/`.

### Tarefa 6 — Uma branch

**Leia:** [Pro Git, branches em poucas palavras](https://git-scm.com/book/pt-br/v2/Ramificação-Branching-no-Git-Branches-em-Poucas-Palavras) e a seção seguinte, de ramificação e mesclagem, no sumário do mesmo capítulo.

**Faça:**

- Crie a branch `experimento`.
- Commite uma frase nova só nela.
- Volte para `main` e confira que a frase não está lá.
- Junte com `git merge experimento`.

**Pronto quando:** `main` tem a frase e `git log --oneline --graph --all` mostra o encontro das duas linhas.

### Tarefa 7 — Conta no GitHub

**Leia:** [Começar no GitHub](https://docs.github.com/pt/get-started).

**Faça:**

- Crie a conta com o e-mail da tarefa 2.
- Ative a verificação que o GitHub pedir.
- Anote o nome de usuário em `git.md`.

**Pronto quando:** você entra em `https://github.com/SEU-USUARIO` e a página abre.

### Tarefa 8 — Chave SSH

**Leia:** [Conectar com SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh), do começo até o teste `ssh -T`.

**Faça:** gere a chave, cadastre a parte `.pub` na conta e rode o teste. A passphrase da chave é uma senha local. Guarde-a.

**Pronto quando:** o teste com `git@github.com` responde que você autenticou.

### Tarefa 9 — Enviar o treino

**Leia:** [Hello World](https://docs.github.com/pt/get-started/start-your-journey/hello-world), a parte de repositório remoto. Crie o repositório vazio no site, sem README automático, para não brigar com o que você já tem.

**Faça:**

```bash
git remote add origin git@github.com:SEU-USUARIO/treino-git.git
git push -u origin main
```

**Pronto quando:** a página do repositório no GitHub mostra `notas.md` e os commits.

### Tarefa 10 — README que outra pessoa entende

**Leia:** [About READMEs](https://docs.github.com/pt/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes) e o [modelo deste plano](../../modelo-readme-atividade.md).

**Faça:** escreva um `README.md` no `treino-git` dizendo o que a pasta é (um treino, não um produto) e como você clonaria. Commit e push.

**Pronto quando:** o GitHub mostra o README formatado na página principal do repositório.

### Tarefa 11 — Clonar em outro lugar

**Faça:**

```bash
cd /tmp
git clone git@github.com:SEU-USUARIO/treino-git.git
cd treino-git
```

- Altere uma linha, commit, push.
- Volte em `~/projetos/treino-git` e rode `git pull`.

**Pronto quando:** as duas pastas têm o mesmo último commit (`git log -1`).

### Tarefa 12 — Mensagens que prestam

**Faça:** olhe `git log` do treino. Reescrever história ainda não é o objetivo. Daqui para a frente, cada mensagem completa a frase "este commit…". Exemplos que servem: `explica stage e commit nas notas`, `ignora rascunhos locais`.

No README, deixe três exemplos de mensagem boa e três ruins, escritos por você.

**Pronto quando:** esses exemplos estão no GitHub.

### Tarefa 13 — Issue

**Leia:** a documentação de issues a partir de [docs.github.com/pt](https://docs.github.com/pt), buscando "Creating an issue".

**Faça:** abra uma issue no `treino-git` com o título "próximo passo do plano" e o corpo apontando a sessão 03. Feche-a com um comentário quando a sessão 03 tiver repositório próprio. Até lá, pode deixar aberta.

**Pronto quando:** a issue existe e o link está nas suas notas.

### Tarefa 14 — O repositório da Estante

**Faça:**

- Crie `~/projetos/estante-html`.
- `git init`, README pelo [modelo](../../modelo-readme-atividade.md), `.gitignore` com pelo menos `.DS_Store` e `*.log` (no Linux o primeiro quase não aparece; manter não atrapalha).
- Repositório vazio no GitHub, remote SSH, push da `main`.

O README descreve a Estante em uma frase: uma coleção pessoal. O tema (livros, jogos, músicas, links) fica escolhido aqui e não muda mais.

**Pronto quando:** `https://github.com/SEU-USUARIO/estante-html` abre o README.

### Tarefa 15 — Atalhos que você vai usar toda semana

**Faça:** nas notas, a sequência exata, testada por você, para o ciclo:

```bash
git status
git diff
git add ARQUIVO
git commit -m "mensagem"
git push
git pull
```

Inclua o que você faz se o `push` for recusado porque o remoto está na frente (`git pull` e só então `git push`).

**Pronto quando:** você executa o ciclo no `estante-html` com uma mudança pequena de verdade, não só a anotação.

### Tarefa 16 — Opcional: desfazer sem pânico

**Leia:** [Pro Git, desfazendo coisas](https://git-scm.com/book/pt-br/v2/Fundamentos-do-Git-Desfazendo-Coisas).

**Faça:** no `treino-git`, altere um arquivo e desfaça a alteração no working tree com o comando que o livro indicar para descartar a mudança local ainda não commitada. Não use `git reset --hard` até ter lido o que ele apaga. Não use `git push --force`.

**Pronto quando:** as notas dizem qual comando você usou e o que ele não recupera.

## Checklist

- [ ] 1. Expliquei os quatro conceitos
- [ ] 2. Configurei nome, e-mail e branch `main`
- [ ] 3. Fiz o primeiro commit
- [ ] 4. Li um `diff` de verdade
- [ ] 5. O `.gitignore` funciona
- [ ] 6. Criei, voltei e juntei uma branch
- [ ] 7. Tenho conta no GitHub
- [ ] 8. SSH testado com sucesso
- [ ] 9. O treino está no GitHub
- [ ] 10. README formatado no site
- [ ] 11. Clonei, alterei e puxei de volta
- [ ] 12. Exemplos de mensagem boa e ruim
- [ ] 13. Uma issue aberta
- [ ] 14. `estante-html` no GitHub
- [ ] 15. Ciclo status/diff/add/commit/push feito na Estante
- [ ] 16. Opcional: desfazer uma mudança local

## Próximo passo

Siga para [03 · HTML e CSS](../03-html-e-css/README.md). O repositório dessa fase é o `estante-html`.
