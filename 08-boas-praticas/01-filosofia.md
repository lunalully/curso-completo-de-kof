# Módulo 08 · Aula 1 — Filosofia: como pensar em Kof

> O objetivo não é aprender a gramática — é aprender **como pensar em Kof**:
> representar o domínio com as abstrações da linguagem, não traduzir Java.

## O "paradigma" da intenção

> **Aviso honesto:** não é um paradigma de verdade. Não existe em catálogo de
> paradigmas, não tem definição formal e ninguém publicou um paper sobre ele.
> É a **orientação a objetos levada ao extremo**: o código expressa *o que*
> quer acontecer, e a plataforma (linguagem + compilador + runtime + stdlib)
> decide *como* — por target, por plataforma, por convenção.

```text
intenção → Kof → compilador → backend
```

| Intenção | Código Kof | O que a plataforma decide |
|----------|-----------|---------------------------|
| "roda isso em paralelo" | `spawn tarefa()` | JVM: virtual threads; Native: pthread |
| "responda /users/:id" | `app.get("/users/:id") { ... }` | servidor HTTP próprio, sem servlet container |
| "mostre uma janela com botão" | `Window`, `Button("+1", () -> ...)` | KofJS renderiza no webview nativo |
| "deserialize isto" | `json.decode<User>(body)` | engine JSON + binding por tipo |
| "cor é vermelha" | `Palette.red` | Int de 32 bits; canais por bitwise |

> Se é essencial para qualquer programa, pertence à plataforma.

### A linha entre intenção e mecanismo

Se um programador precisa conhecer o mecanismo para escrever a intenção, o design falhou:

- `new Thread(...).start()` → rejeitado; escreva `spawn`.
- Anotações + container para HTTP → rejeitado; escreva `app.get(...)`.
- WebView/JavaFX no código de UI → rejeitado; escreva `Window(...)`.

### A separação linguagem ≠ compilador ≠ target (0.3.0+)

```
Linguagem Kof (definida pela especificação)
      │
Kof Compiler (uma implementação)
      │
   Kof IR (máquina de pilha linear, 30 ops)
      │
 JVM | Native (x86_64/riscv64/aarch64) | JS | KofScript (IR) | KofC
```

Ver `docs/language-reference/` (linguagem) e `docs/compiler-architecture.md` (implementação).

### Os 8 princípios da Kof

1. Menos código, mesma capacidade
2. Tipagem forte
3. Intenção acima de cerimônia
4. Um frontend, múltiplos backends
5. Direto para o target
6. Interoperabilidade
7. Sem mágica desnecessária
8. Ferramentas importam

## Gaps nunca são silenciosos

Quando um target não consegue realizar a intenção, ele diz em compile-time
com um código de gap — `CONC001`, `JSN002`, `DB001`, `CONF001`, `SECN001`,
`WEB001`, `LOG001`. Nunca comportamento silenciosamente diferente.

## Complexidade pertence à plataforma

Se a complexidade pode ser absorvida pelo compilador, runtime ou stdlib, ela
desaparece do código do usuário:

```kof
// RUIM — infraestrutura manual
class JsonParser { /* parser manual */ }
class Db { /* conexão manual */ }
class Http { /* servidor manual */ }

// BOM — plataforma
var j = json.encode(user)
var dados = readFile("config.json")
app.get("/x") { return "ok" }
```

## Represente o domínio, não a implementação acidental

```kof
// RUIM — o domínio é "uma sequência", representado como nós encadeados
class Node { Node next; Int value }

// BOM — a abstração da linguagem
class Registry {
    List<LanguageEntry> entries
}
```

## O que Kof NÃO é

- Java com outra sintaxe.
- Kotlin 2.
- Julia para JVM.
- Um transpiler.
- Um gerador de Java.
- Um interpretador fantasiado de compilador (o compilador é real: bytecode/ELF/ESM; o `KofInterpreter` é um target adicional de execução direta, não um disfarce).
- Um clone de Spring.

> **Kof + LLM:** *Human First, LLM Friendly by Consequence.* A consistência (menos ceremony, menos arquivos, menos configuração, mais intenção) faz com que humanos e LLMs entendam a mesma linguagem da mesma forma. O diretório `training/` é parte oficial dessa estratégia.

## Onde aprofundar

- `docs/philosophy.md` — os 8 princípios e a visão de intenção
- `training/idioms/` — FORMA IDIOMÁTICA de cada problema (BAD/GOOD/WHY)
- `training/anti-patterns/` — o que NÃO fazer (fake-idioms, java-like-code, chained-or)
- `learn/` — trilha passo a passo para humanos (00 → 39)

## Exercícios

1. Reescreva estes padrões Java em Kof por intenção: `Thread`, `ObjectMapper`,
   `StringBuilder`, `HashMap`, `Optional`.
2. Para cada um, diga qual abstração da plataforma substitui.
3. (Reflexão) Por que um gap reportado em compile-time é melhor do que
   comportamento "quase funcionando" em runtime?