# Módulo 08 · Aula 3 — Anti-patterns

> Catálogo do que **NÃO** fazer em Kof. Cada um com a alternativa idiomática.

## 1. Código Java-like

Transportar convenções de Java (getters/setters, builders, factories, DTO
ceremony) sem reavaliar se fazem sentido:

```kof
// BAD
class User {
    private String name
    public getName(): String { return name }
    public setName(String name) { this.name = name }
}

// GOOD
class User {
    String name
}
```

**Tabela Java → Kof:**

| Padrão Java | Kof precisa? | Alternativa |
|-------------|--------------|-------------|
| Getter/setter | Não | campo público |
| Builder | Não | construtor/record |
| Factory estática | Não | construtor |
| Utility class static | Não | função top-level |
| Service/Repository/Controller | Não | função ou classe direta |
| StringBuilder | Não | `+` concatena |
| `.equals()` | Não | `==` |
| DTO + mapper | Não | record + `json.encode` |
| Optional | Parcialmente | exceção ou WORKAROUND |

## 2. Estruturas de dados manuais

```kof
// BAD
class Node { String value; Node next }

// GOOD
class Registry {
    List<String> entries
}
```

## 3. Estado duplicado

Guardar `count` ao lado de `items.size`, ou `nomeMaiusculo` ao lado de
`nome` — exige sincronização manual e diverge. **Derive, não armazene.**

## 4. Sentinelas

```kof
// BAD — -1 como "não encontrado"
Int findIndex(String key) { ...; return -1 }

// GOOD — exceção
Int findIndex(String key) { ...; throw "key not found: " + key }
```

> `WORKAROUND` honesto: se o domínio exige ausência como valor, sentinela
> documentada é aceitável **no momento** — mas marque `WORKAROUND`, não idiom.

## 5. Fake idioms (features que não existem)

**Não invente** features de outras linguagens:

| Não existe | Use |
|------------|-----|
| `users.map(u -> u.name)` | `for-in` + `List` |
| `[1, 2, 3]` (array literal) | `new Int[n]` + atribuição |
| `Option.of(x)` | exceção ou WORKAROUND |
| `async`/`await` estilo JS | `spawn f()` + `await r` (JVM) |
| `Thread` | `spawn` |

> `HashMap`/`setOf` de Java não existem — os equivalentes Kof são
> `mapOf()`/`setOf()` (API de métodos: `put/get/contains/remove/size()`).

## 6. Premature optimization

```kof
// BAD — micro-otimização sem necessidade
var buffer = new Char[1024]
var len = 0
for (var i = 0; i < input.length; i = i + 1) {
    buffer[len] = input.charAt(i)
    len = len + 1
}

// GOOD — versão idiomática
var result = ""
for (var i = 0; i < input.length; i = i + 1) {
    result += input.charAt(i)
}
```

**Regra:** escreva idiomático → meça (`now()`, `kof bench`, `kof profile`) →
otimize só o ponto medido, com comentário.

## 7. Abstração desnecessária

Classes "Manager/Helper/Context/Handler/Wrapper" que só repassam chamadas.
Adicione camada apenas quando resolve um problema real (permutabilidade
testada, transação, estado compartilhado).

## 8. Runtime workarounds como regra

`WORKAROUND` legítimo ≠ idiom. Todo workaround carrega o rótulo e cita a
feature planned que o substitui. Nunca apresente workaround como exemplo de
código idiomático.

## 9. Confiar em silêncio (0.5.0 fechou a porta)

Código que "compila mas não faz" era o pior bug: field/método inexistente em
namespace/nome builtin (`math.PI`, `File.bogus()`), cast perdido, branch
engolido. Na 0.5.0 isso é diagnóstico de compile-time (`SEM102` e família):

```kof
// BAD — antes: getfield "?", no-op ou ClassFormatError em runtime
math.PI          // SEM102 (não existe `PI`; use constante sua ou math.*)
File("x").size   // propriedade? use o método documentado

// GOOD — a plataforma diz o que não tem
```

Regra: se algo não está no corpus/training, **compile antes de confiar** —
nunca presuma que uma chamada desconhecida vira no-op inofensivo.

## 10. Null plantado e `Bool?` (SEM048/SEM095)

```kof
// BAD — os dois não existem em Kof moderno
String? s = null
Bool? b = null

// GOOD
String? s = mapOf("k", "v").get("ausente")   // ausência vem da API
Troolean b = desconhecido                    // três estados de verdade
```

## Exercícios

1. Encontre os anti-patterns em um código seu e refatore.
2. Crie uma "checklist de revisão" com os 10 itens acima.
3. (Revisão) Leia `training/anti-patterns/` e anote um exemplo real de cada.