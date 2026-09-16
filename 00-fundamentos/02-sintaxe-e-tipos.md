# Módulo 00 · Aula 2 — Sintaxe e Tipos

> A base de tudo: declarações, tipos, variáveis, operadores e strings.

## Declarações

### Função (sem `fun`)

```kof
// tipo de retorno antes do nome
Int soma(Int a, Int b) {
    return a + b
}

// ou após os parâmetros
soma2(Int a, Int b): Int {
    return a + b
}

// expression body
Bool ehPositivo(Int x) = x > 0
```

### Classe, record e interface

```kof
class User {
    String name
    Int age
}

record Point(Int x, Int y)

interface Speaker {
    speak(): String
}
```

### Enum

```kof
enum Cor { Vermelho, Verde, Azul }
```

Constantes com `==` por conteúdo e API própria:

```kof
var c = Cor.Verde
c.name()                  // "Verde"
println(c)                // Verde
Cor.values()              // List<Cor> — [Vermelho, Verde, Azul]
Cor.valueOf("Azul")       // Cor.Azul (inválido → null)
c == Cor.Verde            // true — comparação por conteúdo
```

Enum brilha com **switch exaustivo** (aula 3): o compilador exige que todos
os casos sejam cobertos (`SEM031`).

### Variável

```kof
var x = 10          // inferência: Int
String nome = "Mel" // explícito
```

## Tipos primitivos

| Tipo | Tamanho | Descrição |
|------|---------|-----------|
| `Bool` | 4 bytes | verdadeiro/falso |
| `Byte` | 1 | inteiro com sinal |
| `Short` | 2 | inteiro com sinal |
| `Int` | 4 | inteiro com sinal |
| `Long` | 8 | inteiro com sinal |
| `Float` | 4 | IEEE 754 |
| `Double` | 8 | IEEE 754 |
| `Char` | 4 | code point UTF-32 |
| `String` | referência | texto imutável |
| `Void` | — | sem retorno |

## Tipos anuláveis (`String?`, `Int?`, etc.)

Desde a 0.2.0, Kof suporta **tipos anuláveis** com sufixo `?`:

```kof
var nome: String? = null        // pode ser null
var idade: Int? = 25            // pode ser null ou Int

// guarda de null (narrowing) — após o check, o tipo vira não-anulável
if (nome != null) {
    println(nome.length)        // seguro: nome é String aqui
} else {
    println("sem nome")
}
```

- `kof check` exige o `if (x != null)` antes de dereferenciar.
- No runtime, `String?` faz erasure para `String` (mesma representação).
- Interop Java: métodos que podem retornar `null` mapeiam para `String?`.

## Operadores

```kof
// aritmética
a + b; a - b; a * b; a / b; a % b

// comparação (retorna Bool)
a == b; a != b; a < b; a > b; a <= b; a >= b

// lógicos
a && b; a || b; !a

// concatenação de strings
"Hello" + " World"      // "Hello World"
```

**Atenção:** `==` em Kof compara **conteúdo**, inclusive para strings. Não
use `.equals()` (hábito de Java) — não é idiomático.

## Conversões

- *Widening* automático: `Int → Long → Float → Double`.
- `String + qualquer coisa → String`.
- Comparações e lógicos produzem `Bool`.

## Arrays

```kof
var arr = new Int[5]     // array de 5 inteiros (zerados)
arr[0] = 10
println(arr[0])          // 10
println(arr.length)      // 5
```

- Bounds são verificados em runtime (`array index out of bounds`).
- **Não existe** literal `{1, 2, 3}` — use `new Int[n]` + atribuição,
  ou `listOf(...)` para coleção dinâmica.

## Strings

```kof
var s = "Hello World"
s.length                    // 11
s.charAt(0)                 // 72 (valor numérico de 'H')
s.substring(0, 5)           // "Hello"
s.substring(6)              // "World"
s.contains("World")         // true
s.startsWith("Hello")       // true
s.endsWith("orld")          // true
s.indexOf("W")              // 6
s.toUpperCase()             // "HELLO WORLD"
s.toLowerCase()
s.trim()
s.split(" ")                // String[]
s == "Hello World"          // true (conteúdo)
```

**Correção 0.3.1 (bug 43):** `String.length` agora conta **unidades UTF-16** nos 5 alvos (paridade JVM/JS/Native/riscv/aarch64; `café` → 4, `a😀b` → 4 por STR001). `charAt` idem.

> `fn`/`fun`/`func` são **palavras reservadas** (SG-001) — não existem em nenhuma posição. `let`/`const` não existem (`PARSE085`); use `var`/`val`.

## Novidades 0.3.5–0.4.x (kof.strings / kof.encoding / arrays multidim / stdlib)

```kof
// kof.strings — 5 alvos
strings.escapeJson("a\"b")   // "a\"b" (RFC 8259)
strings.unescapeHtml("&lt;b&gt;") // "<b>"
strings.toCamelCase("hello-world") // "helloWorld"
strings.indent("ab", 2)     // recua 2 espaços (0.4.x)

// kof.encoding — 5 alvos
encoding.base64Encode("oi") // "b2k="
encoding.hexEncode("oi")    // "6f69"

// arrays multidimensionais — 5 alvos
var m = new Int[2][3]
println(m[0].length)        // 3

// kof.math Double — 5 alvos (0.4.x)
println(math.roundTo(3.14159, 3))   // 3.142 (meio-para-longe-do-zero)
println(math.parseIntOrDefault("abc", 42)) // 42 (falha DEVOLVE o default)

// kof.uuid — v4 + v7 ordenado no tempo (RFC 9562) + isUuid
println(uuid.v7().length)   // 36

// kof.time — calendário civil ISO (0.4.x, 5 alvos)
println(time.addDays("2026-09-15", 1)) // "2026-09-16"
```

## Contratos de rejeição (0.4.x — paridade absoluta, nunca silêncio)

O compilador rejeita construtos que divergiam por target. Não são bugs — são
a linguagem escolhendo um contrato único (ver notas):

```kof
// String: ordem lexicográfica é compareTo, não `<` (SEM053)
"abc".compareTo("abd") < 0     // true — o caminho idiomático
// "abc" < "abd"               // COMPILE ERROR [SEM053]

// split devolve String[] (array fixo), não List: indexe com colchetes
var ps = "a,b,c".split(",")
println(ps[0])                 // "a"
println(ps.length)             // 3
// ps.get(0)                   // COMPILE ERROR [SEM028] (array não tem .get)

// subscript `[]`: array e List aceitam; String/Map/Set rejeitam (SEM054)
var l = listOf(1, 2, 3)
println(l[0])                 // 1 (List aceita)
println(l.get(0))            // 1 (idiomático)
// "abc"[0]                   // COMPILE ERROR [SEM054] — use charAt(i)
// mapOf("a",1)["a"]          // COMPILE ERROR [SEM054] — use get(k)

// Long é 64-bit real em todos os alvos (no JS, BigInt — decisão 5b)
var n = 9223372036854775807
println(n / 2)                // 4611686018427387903
```

## Limitações reais (não invente)

- `replace` existe como `replace(Char, Char)` e `replace(String, String)` (0.3.0+ cross).
- `split` retorna `String[]` e **continua regex**: `"a|b".split("|")` divide
  carácter-a-carácter — use `split("\\|")`.
- `Map.get` devolve `V?` (nullable) sempre — narrowing `if (v != null)` obrigatório.
- Não existe `StringBuilder` — use `+` (concatenar é intenção).
- `fn`/`fun`/`func`/`let`/`const` não existem na linguagem.

## Exercícios

1. Escreva uma função que recebe `String` e devolve o número de vogais.
2. Escreva uma função que devolve `true` se uma string é palíndroma.
3. Crie um array de 10 `Int`, preencha com os quadrados de 1..10 e imprima.