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

Desde a 0.2.0, Kof suporta **tipos anuláveis** com sufixo `?`. O `null` é
**intenção explícita de ausência** — ele chega pelas APIs (ex.: `Map.get`),
nunca como literal plantado no código:

```kof
// null vem da API (SEM048 desde a 0.3.22: `var x: String? = null` é erro)
var nome: String? = mapOf("a", "Mel").get("b")   // "b" não existe → null
var idade: Int? = mapOf("Mel", 25).get("Mel")    // 25

// guarda de null (narrowing) — após o check, o tipo vira não-anulável
if (nome != null) {
    println(nome.length)        // seguro: nome é String aqui
} else {
    println("sem nome")
}
```

- `kof check` exige o `if (x != null)` antes de dereferenciar (o narrowing vale
  **só dentro do ramo**; fora dele o tipo continua `T?`).
- `null` **literal** como valor (`=` ou `return` em função anulável é a única
  exceção) é rejeitado com `SEM048` — o erro ensina: *null safety funciona por
  narrowing, nunca por literais*.
- `Bool?`/`Boolean?` **não existem** (`SEM095`). Três estados são `Troolean`.
- No runtime, `String?` faz erasure para `String` (mesma representação).
- Interop Java: métodos que podem retornar `null` mapeiam para `String?`.

### Troolean — true / false / desconhecido (0.5.0)

`Bool` tem exatamente dois valores. Para lógica de três estados use `Troolean`
(Kleene: `F` domina `&&`, `T` domina `||`, `!desconhecido = desconhecido`):

```kof
resposta(Bool certo): Troolean { return certo }

main() {
    Troolean desconhecido          // declarado e não instanciado = desconhecido
    println(desconhecido == true)  // false — comparação separa os três estados
    println(desconhecido)          // null (imprime true/false/null)

    var r = resposta(false) && desconhecido   // false — F domina o AND
    println(r)
}
```

Consuma como `Bool` com narrowing: `if (t == true)` ou `if (t != null)`.

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
- **Casts saturantes (0.4.0):** `Double`/`Float as Int`/`Long` satura no limite
  (JLS 5.1.3): `NaN → 0`, `3.9 → 3`, `1e30 as Int → Int.MAX_VALUE`.
- `"2026" as Int` **não converte texto** (`SEM100`) — use `math.parseInt(s)`.
- **Invariança de generics (0.5.0):** `List<Int>` **não** é atribuível a
  `List<String>` (erro em compile-time, `SEM012`/`SEM021`). Sem
  `List<Int> → List<String>` silencioso com `ClassCastException` depois.
- No JS, `Long` é `BigInt` de verdade (paridade real de 64 bits, 0.4.0).

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
s.charAt(0)                 // 'H' (Char; como Int vale 72)
s.substring(0, 5)           // "Hello"
s.toCharArray()             // Char[] — 5 alvos (0.5.0)
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

## Novidades 0.3.5–0.3.7 (kof.strings / kof.encoding / arrays multidimensionais)

```kof
// kof.strings — 5 alvos
strings.escapeJson("a\"b")   // "a\"b" (RFC 8259)
strings.unescapeHtml("&lt;b&gt;") // "<b>"
strings.toCamelCase("hello-world") // "helloWorld"
strings.uncapitalize("Hello")     // "hello" (0.3.22)
strings.reverse("café")           // "éfac" — por code point (0.5.0)
strings.isAlpha("abc")            // true — predicados ASCII

// kof.encoding — 5 alvos
encoding.base64Encode("oi") // "b2k="
encoding.base64UrlEncode("oi") // "b2k" — alfabeto URL, sem padding
encoding.hexEncode("oi")    // "6f69"

// arrays multidimensionais — 5 alvos
var m = new Int[2][3]
println(m[0].length)        // 3
```

## Novidades 0.3.8–0.5.0 (o que mudou de verdade)

**Stdlib que cresceu (tudo verificado nos alvos):**

```kof
// kof.math — Double explícito (não aceita Int sem cast)
math.sqrt(81.0)             // 9.0 (NaN para negativo, IEEE)
math.roundTo(3.14159, 2)    // 3.14
math.lerp(0.0, 10.0, 0.5)   // 5.0
math.parseInt("42")         // 42 (SEM100: `"42" as Int` não converte texto)
math.parseDouble("2.5")     // 2.5
math.pow(2.0, 10.0)         // 1024.0 — 5 alvos (cross fechado na 0.5.0)

// kof.time — calendário em String/Int puro (sem objeto de data)
time.todayIso()                          // "2026-09-27"
time.addDays("2026-01-31", 1)            // "2026-02-01"
time.diffDays("2026-01-01", "2026-01-31") // 30
time.isLeapYear(2026)                    // false

// kof.uuid / kof.random / kof.validation
uuid.v7()                          // ordenado por tempo (RFC 9562)
uuid.isUuid(id)                    // valida o shape, não a versão
random.randomBytesHex(16)          // 32 chars hex (tokens)
validation.formatCpf("12345678901") // "123.456.789-01" (formata, não valida)
validation.isNis("123...")          // predicado strict

// rng — PRNG semeado e REPRODUTÍVEL (para testes, ver módulo 11)
rng.seed(42)
rng.int(100)                       // mesma seed = mesma sequência em todo alvo
```

**Mudanças de comportamento que quebram código antigo:**

- `null` literal → `SEM048` (já explicado acima).
- `Bool?` → `SEM095`; use `Troolean`.
- `enum == String` → `SEM062` (compare com a constante, não com texto).
- Generics invariantes: `List<Int>` não vira `List<String>` (0.5.0).
- `s[0]` em `String`/List/Map/Set é **rejeitado** (`SEM054`): use
  `charAt(0)`/`get(0)` — a intenção do índice precisa ser explícita.
- `return <valor>` em função `void`/construtor → `SEM093`; use `return` puro.

## Limitações reais (não invente)

- `replace` existe como `replace(Char, Char)` e `replace(String, String)` (0.3.0+ cross).
- `split` retorna `String[]`.
- Não existe `StringBuilder` — use `+` (concatenar é intenção).
- `s[0]` **não** indexa String/List/Map (`SEM054`): use `charAt`/`get`.
  Arrays são a única coleção indexável com `[]`.
- `matches`/`replaceAll`/`replaceFirst`/`compareToIgnoreCase` são `STR003`
  (recusa honesta em compile-time) fora da JVM. Para contar ocorrências use
  `strings.count(texto, sub)`; para trocar, `replace`.

## Exercícios

1. Escreva uma função que recebe `String` e devolve o número de vogais.
2. Escreva uma função que devolve `true` se uma string é palíndroma.
3. Crie um array de 10 `Int`, preencha com os quadrados de 1..10 e imprima.