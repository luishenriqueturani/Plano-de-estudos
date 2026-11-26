# Sessão 01 — Terminal no Linux (CachyOS)

**Sessão 1 de 21 · cerca de 1 semana**

O terminal é a ferramenta das próximas vinte sessões. Git, nvm, Docker e os bancos vão ser usados por comando. Esta semana deixa esses comandos familiares antes de aparecer programação.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **Próxima →** [02 · Git e GitHub](../02-git-e-github/README.md)

## O que você vai saber fazer

- Entrar numa pasta, criar outra, mover e apagar com intenção.
- Ler e criar um arquivo de texto pelo terminal.
- Instalar `git` e `curl` com o `pacman`.
- Pedir ajuda ao próprio comando e à wiki, em vez de decorar lista.

## Documentação

- [Pacman — wiki do Arch](https://wiki.archlinux.org/title/Pacman)
- [Pacman/Rosetta](https://wiki.archlinux.org/title/Pacman/Rosetta) — se você já viu `apt` em algum tutorial e quer o equivalente
- [Ajuda: leitura](https://wiki.archlinux.org/title/Help:Reading) — como ler uma página da wiki, inclusive o que significa `#` e `$`
- [Core utilities — wiki do Arch](https://wiki.archlinux.org/title/Core_utilities)
- [Documentação do CachyOS](https://wiki.cachyos.org/)

O CachyOS usa os mesmos pacotes e o mesmo `pacman` do Arch. Um tutorial escrito para Ubuntu (`apt`) não vale aqui.

## Tarefas

### Tarefa 1 — Descobrir onde você está

**Leia:** a seção de convenções em [Help:Reading](https://wiki.archlinux.org/title/Help:Reading) (o prompt `$` é o seu usuário; `#` é o root).

**Faça:**

- Abra o terminal.
- Rode `whoami`, `pwd`, `echo $SHELL` e `ls`.
- Anote num arquivo `notas-terminal.md` o usuário, a pasta atual e o shell (`bash`, `zsh` ou `fish`).

**Pronto quando:** o arquivo existe e você explica, sem olhar, a diferença entre a pasta pessoal e `/`.

### Tarefa 2 — Navegar

**Faça:**

- Vá para a pasta pessoal com `cd`.
- Entre em `Documentos`, `Downloads` ou qualquer pasta que já exista.
- Volte um nível com `cd ..`.
- Use `ls -la` e identifique o que é arquivo e o que é pasta.

**Pronto quando:** você chega em `~/projetos` mesmo que ela ainda não exista (a próxima tarefa cria).

### Tarefa 3 — Criar a árvore de projetos

**Faça:**

```bash
mkdir -p ~/projetos/rascunhos
cd ~/projetos
pwd
```

- Crie também `~/projetos/notas`.
- Mova `notas-terminal.md` para `~/projetos/notas/`.

**Pronto quando:** `ls ~/projetos` mostra `notas` e `rascunhos`.

### Tarefa 4 — Copiar, renomear e apagar com cuidado

**Faça:**

- Copie o arquivo de notas.
- Renomeie a cópia.
- Crie uma pasta `lixo` dentro de `rascunhos`, mova a cópia para lá e apague só essa pasta com `rm -r`.
- Antes do `rm`, rode `ls` no caminho completo. Apague apenas o que você acabou de criar.

**Pronto quando:** o original em `~/projetos/notas/` continua no lugar e a cópia não existe mais.

### Tarefa 5 — Ler e editar um texto

**Faça:**

- Veja o conteúdo com `cat`.
- Abra o arquivo no editor que você já usa, ou instale um editor de terminal: `sudo pacman -S nano`, e edite com `nano`.
- Acrescente uma seção "comandos que eu já usei", com uma linha por comando e o que ele fez.

**Pronto quando:** o arquivo abre de novo e a seção nova está salva.

### Tarefa 6 — Ajuda do próprio comando

**Faça:**

- Rode `ls --help` e `mkdir --help`.
- Abra `man ls` e ache a explicação de `-a`. Saia do `man` com `q`.
- Escreva nas notas o que `-a` muda.

**Pronto quando:** as notas têm essa explicação com as suas palavras.

### Tarefa 7 — Instalar git e curl

**Leia:** [Pacman](https://wiki.archlinux.org/title/Pacman), seções de sincronizar a base e instalar pacotes.

**Faça:**

```bash
sudo pacman -Syu
sudo pacman -S git curl
git --version
curl --version
```

`pacman -Syu` atualiza o sistema. É o hábito desta distribuição antes de instalar pacote. Se o comando pedir para confirmar, leia o que será instalado.

**Pronto quando:** os dois `--version` imprimem um número.

### Tarefa 8 — Variáveis e caminho

**Faça:**

- Rode `echo $HOME` e `echo $PATH`.
- Rode `which git` e `which curl`.
- Nas notas, explique em duas frases o que o `$PATH` resolve.

**Pronto quando:** você aponta a pasta onde o `git` foi instalado.

### Tarefa 9 — Encadear comandos simples

**Faça:**

- Liste só os nomes de `~/projetos` e grave essa lista num arquivo com `ls > ~/projetos/notas/lista.txt`.
- Conte as linhas com `wc -l`.
- Procure a palavra "git" nas suas notas com `grep`.

**Pronto quando:** `lista.txt` existe e o `grep` acha a linha em que você anotou o `git`.

### Tarefa 10 — Página da wiki, de verdade

**Leia:** [Help:Reading](https://wiki.archlinux.org/title/Help:Reading) até o fim das convenções de comando.

**Faça:** escreva nas notas o que a wiki quer dizer quando um bloco começa com `#` e quando começa com `$`.

**Pronto quando:** essa nota existe e bate com a wiki, não com um chute.

## Checklist

- [ ] 1. Localizei usuário, pasta e shell
- [ ] 2. Naveguei com `cd` e `ls`
- [ ] 3. Criei `~/projetos`
- [ ] 4. Copiei, renomeei e apaguei só o que criei
- [ ] 5. Editei um texto e salvei
- [ ] 6. Usei `--help` e `man`
- [ ] 7. Instalei `git` e `curl`
- [ ] 8. Li `$HOME`, `$PATH` e `which`
- [ ] 9. Usei redirecionamento e `grep`
- [ ] 10. Sei ler `#` e `$` na wiki

## Próximo passo

Siga para [02 · Git e GitHub](../02-git-e-github/README.md).
