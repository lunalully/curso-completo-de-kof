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

**Diferença por target:** no Native, `length` conta **bytes UTF-8**; no JVM,
conta **unidades UTF-16**. Não assuma contagem de caracteres para strings com
acentos quando o target importa.

## Limitações reais (não invente)

- `replace` só existe como `replace(Char, Char)`.
- `split` retorna `String[]`.
- Não existe `StringBuilder` — use `+` (concatenar é intenção).

## Exercícios

1. Escreva uma função que recebe `String` e devolve o número de vogais.
2. Escreva uma função que devolve `true` se uma string é palíndroma.
3. Crie um array de 10 `Int`, preencha com os quadrados de 1..10 e imprima.