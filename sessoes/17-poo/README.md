# Sessão 17 — Programação orientada a objetos

**Sessão 17 de 28 · cerca de 3 semanas**

Até aqui a Estante foi feita de funções e de objetos soltos. O NestJS, na próxima sessão, é feito de classes: controller, service, guard, entidade. Cada um nasce com `new`, guarda estado e recebe os outros pelo construtor. Quem chega lá sem ter escrito isso na mão lê `@Injectable()` como feitiço.

Esta sessão monta o mesmo desenho em TypeScript puro, sem Nest. O repositório é pequeno de propósito. O que importa é a forma que a API vai repetir.

> **Trilha:** [Índice](../../README.md) · [Como estudar](../../COMO-ESTUDAR.md) · [Progresso](../../PROGRESSO.md)
>
> **← Anterior:** [16 · Next.js](../16-nextjs/README.md)
> **Próxima →** [18 · NestJS](../18-nestjs/README.md)

## O que você vai saber fazer

- Separar a classe (o molde) da instância (o item concreto).
- Esconder o estado e mudar o item só por método.
- Trocar um repositório por outro sem reescrever o service.
- Entregar ao service, pelo construtor, o objeto de que ele precisa.
- Reconhecer, no Nest, classe, método e injeção como coisas que você já escreveu.

## Documentação

- [Classes no TypeScript](https://www.typescriptlang.org/docs/handbook/2/classes.html) — construtor, `private`, `implements` e `this`
- [Classes no JavaScript](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript)
- [Referência de classes](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes)
- [Decorators](https://www.typescriptlang.org/docs/handbook/decorators.html) — leia para reconhecer o `@` do Nest. Não instale o Nest nesta sessão

## Tarefas

Repositório `estante-objetos`. TypeScript em modo estrito, como na sessão 15, e testes com `node:test`. Sem banco, sem HTTP, sem framework.

### Tarefa 1 — Molde e objeto

**Faça:** uma classe `Item` com `titulo` e `estado`. Crie dois itens. Altere o título de um. Mostre, num teste, que o outro permanece como nasceu.

**Pronto quando:** o teste falharia se os dois compartilhasse o mesmo objeto. No README, uma frase distingue a classe da instância.

### Tarefa 2 — Construtor e método

**Faça:** o construtor recebe título e estado inicial. O método `avancar()` percorre `planejado` → `fazendo` → `feito`. Fora dessa ordem, o método lança um erro. O estado é o mesmo tipo união da sessão 15.

**Pronto quando:** três testes cobrem um avanço válido, o avanço a partir de `feito` e um título vazio recusado no construtor.

### Tarefa 3 — `this`

**Faça:** guarde `item.avancar` numa variável e chame essa variável. Anote o erro. Corrija do jeito que a documentação de classes mostrar para manter o `this` no item (campo com arrow function, ou `bind`). Escreva qual você escolheu.

**Pronto quando:** a chamada solta funciona depois da correção, e o README diz o que o `this` apontava antes.

### Tarefa 4 — Estado privado

**Faça:** `estado` deixa de ser público. Quem está fora da classe lê por um getter e muda só com `avancar()`. Uma atribuição direta ao campo precisa falhar na checagem de tipos.

**Pronto quando:** o programa que compila não tem como pular um estado. O teste de avanço continua passando pelo método.

### Tarefa 5 — Interface e classe

**Faça:** uma interface `RepositorioItem` com `salvar` e `listar`. A classe `RepositorioMemoria` implementa essa interface e guarda os itens num array privado.

**Pronto quando:** uma função que recebe `RepositorioItem` compila com a memória. O array não aparece no tipo público.

### Tarefa 6 — Herança que não cabe

**Faça:** crie `class Nota extends Item`, use por alguns minutos e apague. No README, diga o que uma nota tem (texto, item a que pertence) e o que ela não tem (estado de leitura da coleção). Herança fica reservada para quando um tipo for de fato um caso do outro.

**Pronto quando:** o `extends` saiu do código e o parágrafo ficou no README. Não precisa de uma hierarquia de classes no repositório.

### Tarefa 7 — Composição

**Faça:** uma classe `Colecao` com um array privado de `Item`. Métodos `adicionar` e `listar`. A coleção contém itens. Ela não é um item e não estende `Item`.

**Pronto quando:** um teste adiciona dois itens e lista os dois, e `Colecao` não aparece como subtipo de `Item`.

### Tarefa 8 — O mesmo método, outro objeto

**Faça:** uma segunda classe que implementa `RepositorioItem` e falha de propósito ao salvar (disco cheio, por exemplo, com um erro seu). Uma função `gravar(repo, item)` usa só a interface. O teste chama `gravar` com a memória e com a classe que falha.

**Pronto quando:** `gravar` não menciona o nome das duas classes. Quem muda é o objeto passado.

### Tarefa 9 — Dependência pelo construtor

**Faça:** `class ItensService { constructor(private readonly repo: RepositorioItem) {} }` com `criar(titulo)`, que monta o item e chama `repo.salvar`. O service não contém `new RepositorioMemoria()`. Quem chama o service é que escolhe o repositório e faz o `new`.

**Pronto quando:** o service compila recebendo qualquer `RepositorioItem`. A palavra `new`, para o repositório, aparece no teste ou num `main`, não dentro do service.

### Tarefa 10 — Teste com um repositório falso

**Faça:** no teste do service, passe uma classe pequena que implementa a interface e só registra o que foi salvo. Confira que `criar` gravou um item com o título pedido. Nenhum array do service é consultado, porque o service não tem esse array.

**Pronto quando:** o teste passa sem `RepositorioMemoria`. Dá para ler, no teste, o que o service pediu para salvar.

### Tarefa 11 — O `@` que o Nest vai usar

**Faça:** leia a página de decorators o suficiente para escrever cinco linhas no README. Um decorator é uma função aplicada a uma classe ou a um método. No Nest, `@Controller()` e `@Injectable()` marcam a classe para o framework saber o que criar e o que passar no construtor. Você ainda não escreve um decorator. Anote que a documentação separa o decorator antigo, que o Nest usa, do decorator novo da linguagem.

**Pronto quando:** o README tem essas cinco linhas com o link da página que você leu. O `package.json` deste repositório não tem `@nestjs`.

### Tarefa 12 — Mapa para a próxima sessão

**Faça:** uma tabela no README.

| Aqui | No Nest |
| --- | --- |
| `ItensService` | provider, a classe com a regra |
| `constructor(repo)` | injeção de dependência |
| `RepositorioItem` | o contrato que depois o TypeORM cumpre |
| `Item` | a classe de domínio; a entidade de tabela vem na sessão 19 |
| você chama `new` | o Nest chama `new` a partir do módulo |

**Pronto quando:** a tabela está preenchida com os nomes dos seus arquivos.

## Checklist

- [ ] 1. Duas instâncias independentes
- [ ] 2. `avancar()` com testes
- [ ] 3. `this` corrigido e explicado
- [ ] 4. Estado privado
- [ ] 5. Interface implementada pela memória
- [ ] 6. `extends` experimentado e removido
- [ ] 7. Coleção contém itens
- [ ] 8. `gravar` aceita os dois repositórios
- [ ] 9. Service recebe o repositório
- [ ] 10. Teste com repositório falso
- [ ] 11. Decorator explicado, sem Nest instalado
- [ ] 12. Tabela ligando este código ao Nest

## Próximo passo

Siga para [18 · NestJS](../18-nestjs/README.md). Leve o `ItensService`: lá o framework passa a chamar o `new` que nesta sessão ainda é seu.
