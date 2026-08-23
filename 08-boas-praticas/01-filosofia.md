# Módulo 08 · Aula 1 — Filosofia: como pensar em Kof

> O objetivo não é aprender a gramática — é aprender **como pensar em Kof**:
> representar o domínio com as abstrações da linguagem, não traduzir Java.

## O "paradigma" da intenção

Kof é orientada à **intenção**: o código expressa *o que* quer, e a plataforma
decide *como*.

```text
intenção → Kof → compilador → backend
```

| Intenção | Em vez de |
|----------|-----------|
| `spawn tarefa()` | `new Thread(...).start()` |
| `app.get("/users/:id")` | servlet container |
| `Window` / `Button("+1", () -> ...)` | WebView/JavaFX |
| `json.decode<User>(body)` | parser manual |
| `Palette.red` | `0xFF0000FF` |

> Se é essencial para qualquer programa, pertence à plataforma.

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
- Um transpiler.
- Um gerador de Java.
- Um interpretador fantasiado de compilador.
- Um clone de Spring.

## Exercícios

1. Reescreva estes padrões Java em Kof por intenção: `Thread`, `ObjectMapper`,
   `StringBuilder`, `HashMap`, `Optional`.
2. Para cada um, diga qual abstração da plataforma substitui.
3. (Reflexão) Por que um gap reportado em compile-time é melhor do que
   comportamento "quase funcionando" em runtime?