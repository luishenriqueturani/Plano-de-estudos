# Sessão 05 — Ambiente Node.js e nvm

**Sessão 5 de 28 · cerca de 1 semana**

O Node é o JavaScript fora do navegador. O nvm instala a versão que o projeto pede e permite trocar depois, sem misturar com o pacote da distribuição. Nesta semana você instala, confere e deixa registrado. A lógica maior fica na sessão 06.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [04 · JavaScript no navegador](../04-javascript-no-navegador/README.md)
> **Próxima →** [06 · Node.js e lógica](../06-nodejs-e-logica/README.md)

## O que você vai saber fazer

- Instalar o nvm no CachyOS, no shell que você realmente usa.
- Instalar a versão LTS do Node e uma segunda versão.
- Fixar a versão de um projeto com `.nvmrc`.
- Criar um `package.json` e rodar um script.

## Documentação

- [nvm, instalação e atualização](https://github.com/nvm-sh/nvm#installing-and-updating)
- [Node.js — wiki do Arch](https://wiki.archlinux.org/title/Node.js), seção *Node version manager*
- [Introdução ao Node](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)
- [Como versionar pacotes Node](https://nodejs.org/en/learn/getting-started/an-introduction-to-the-npm-package-manager) — se o artigo tiver outro título, comece por [nodejs.org/en/learn](https://nodejs.org/en/learn)

Não instale o Node pelo `pacman` (`nodejs`) ao mesmo tempo em que usa o nvm. As duas instalações disputam o comando `node`. O caminho deste plano é o nvm.

## Tarefas

### Tarefa 1 — Por que a versão importa

**Leia:** a introdução do [README do nvm](https://github.com/nvm-sh/nvm).

**Faça:** em `~/projetos/notas/node.md`, explique por que um projeto pode precisar do Node 22 e outro do 24, e por que isso não deve depender de atualizar o sistema inteiro.

**Pronto quando:** a nota tem um exemplo concreto, mesmo que imaginado.

### Tarefa 2 — Shell de novo

**Faça:** rode `echo $SHELL`.

- `bash` ou `zsh`: siga o script oficial na tarefa 3.
- `fish`: o script oficial do nvm não configura o fish. Instale o pacote da distribuição e siga a wiki na tarefa 4.

**Pronto quando:** a nota diz qual caminho você vai usar e por quê.

### Tarefa 3 — nvm pelo script oficial (bash ou zsh)

**Leia:** [Installing and Updating](https://github.com/nvm-sh/nvm#installing-and-updating). Copie o comando `curl` de lá, no dia em que for instalar. Não use uma cópia antiga espalhada por blog: a versão do script muda.

**Faça:**

- Rode o comando de instalação.
- Feche o terminal e abra de novo, ou carregue o arquivo que o instalador indicar (`~/.bashrc` ou `~/.zshrc`).
- `command -v nvm` precisa responder `nvm`.

**Pronto quando:** `nvm --version` imprime um número.

### Tarefa 4 — nvm pelo pacote do CachyOS (fish, ou se o script não couber)

**Leia:** [Node.js na wiki do Arch](https://wiki.archlinux.org/title/Node.js), seção *Node version manager*.

**Faça:**

```bash
sudo pacman -S nvm
```

Carregue o nvm do jeito que a wiki manda para o seu shell. No bash e no zsh isso costuma ser:

```bash
source /usr/share/nvm/init-nvm.sh
```

Essa linha precisa estar no arquivo de início do shell, senão o próximo terminal "perde" o nvm. No fish, use o método que a wiki descrever para esse shell, inclusive um helper se ela pedir.

**Pronto quando:** um terminal novo, aberto do zero, ainda encontra `nvm`.

Faça a tarefa 3 ou a 4, não as duas.

### Tarefa 5 — Instalar a LTS e usar

**Faça:**

```bash
nvm install --lts
nvm alias default 'lts/*'
node -v
npm -v
which node
```

**Pronto quando:** `which node` aponta para uma pasta dentro do nvm (em geral `~/.nvm/...`) ou para o caminho que a instalação via pacote documenta. Não aponta para `/usr/bin/node`.

### Tarefa 6 — Segunda versão

**Faça:**

- Instale também a versão "current" ou uma LTS anterior, com o comando que `nvm ls-remote --lts` te ajudar a escolher.
- `nvm use VERSAO` e confira `node -v`.
- Volte para a LTS com `nvm use default`.

**Pronto quando:** você troca de versão e o `node -v` muda, no mesmo terminal.

### Tarefa 7 — `.nvmrc`

**Leia:** a parte de `.nvmrc` no README do nvm.

**Faça:** em `~/projetos/rascunhos/node-versao`, crie `.nvmrc` com a versão LTS que você adotou (o número que `node -v` mostrou, sem o `v`, ou com o `v`: confira o que o seu nvm aceita). Entre na pasta e rode `nvm use`.

**Pronto quando:** `nvm use` sem argumento lê o arquivo e seleciona essa versão.

### Tarefa 8 — Primeiro processo Node

**Leia:** [Introduction to Node.js](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs).

**Faça:** `ola.js` que imprime `process.version` e o primeiro argumento da linha de comando (`process.argv`). Rode `node ola.js estante`.

**Pronto quando:** a saída mostra a versão e a palavra `estante`.

### Tarefa 9 — package.json

**Faça:**

```bash
mkdir ~/projetos/estante-cli
cd ~/projetos/estante-cli
git init
npm init -y
```

Edite o `package.json`: nome, descrição de uma linha, `"private": true` se não for publicar no npm. Crie o script `"start": "node ola.js"`. O `ola.js` pode só dizer que a CLI ainda vai nascer na sessão 06.

**Pronto quando:** `npm start` roda, e existe um commit inicial. O repositório remoto pode esperar a sessão 06, mas o `git init` é agora.

### Tarefa 10 — O que não vai para o Git

**Faça:** `.gitignore` com `node_modules/`. Instale um pacote minúsculo só para ver a pasta nascer (`npm install cowsay` serve) e confira que `git status` não lista `node_modules`. Pode remover o pacote depois com `npm uninstall cowsay`.

**Pronto quando:** o `.gitignore` está commitado e `node_modules` não aparece no `git status`.

### Tarefa 11 — Anotar a instalação

**Faça:** no README de `estante-cli`, a seção "ambiente" com:

- shell usado;
- nvm pelo script ou pelo pacote;
- versão LTS escolhida;
- os comandos `nvm install` e `nvm use` que você rodou.

**Pronto quando:** outro leitor com CachyOS consegue repetir, usando a documentação linkada e as suas notas.

## Checklist

- [ ] 1. Sei por que a versão fica por projeto
- [ ] 2. Identifiquei o shell
- [ ] 3 ou 4. nvm instalado por um dos caminhos
- [ ] 5. `node` e `npm` vêm do nvm
- [ ] 6. Troquei de versão e voltei
- [ ] 7. `.nvmrc` funciona
- [ ] 8. Script com `process.argv`
- [ ] 9. `package.json` com `npm start`
- [ ] 10. `node_modules` ignorado
- [ ] 11. README com o passo a passo da minha máquina

## Próximo passo

Siga para [06 · Node.js e lógica de programação](../06-nodejs-e-logica/README.md). O projeto continua em `estante-cli`.
