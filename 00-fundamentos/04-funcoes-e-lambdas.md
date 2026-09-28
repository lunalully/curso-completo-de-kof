# Módulo 00 · Aula 4 — Funções e Lambdas

> Funções são cidadãs de primeira classe — sem `fun`, sem cerimônia.

## Formas válidas de função

```kof
// entry point
main() {
    println("oi")
}

// tipo antes do nome
String saudacao() {
    return "oi"
}

// tipo após os parâmetros
despedida(): String {
    return "tchau"
}

// void explícito
void fazAlgo() {
    println("ok")
}

// expression body
Int dobro(Int x) = x * 2

// parâmetros e retorno
Int soma(Int a, Int b) {
    return a + b
}
```

## Quando usar função top-level

- Lógica sem estado: helpers, validação, transformação.
- A utility class de Java vira **função top-level** em Kof.

```kof
// RUIM (herança de Java)
class StringUtils {
    static String capitalizar(String s) {
        return s.substring(0, 1).toUpperCase() + s.substring(1)
    }
}

// BOM — função top-level
String capitalizar(String s) {
    return s.substring(0, 1).toUpperCase() + s.substring(1)
}
```

Kof tem funções fora de classes — a camada extra é ruído.

## Lambdas

```kof
var f = (x: Int) -> x * 2
println(f(21))              // 42

var g = (a: Int, b: Int) -> a + b
println(g(3, 4))            // 7

var h = () -> 99
println(h())                // 99
```

Lambdas compilam para classes sintéticas com um método `invoke`.

**Capturas (0.3.0+):** lambdas capturam variáveis do escopo — inclusive com parâmetros tipados e em 3 níveis de aninhamento. Capturas mutáveis usam `Box` (`mutação fora reflete dentro`):

```kof
class Contador {
    Int n
}

main() {
    var c = Contador()
    c.n = 0
    var inc = () -> {
        c.n = c.n + 1
        return c.n
    }
    inc()
    inc()
    println(c.n)            // 2
}

var offset = 10
var f = (x: Int) -> x + offset   // captura + param tipado ✅ (0.3.0)
offset = 20
println(f(5))                // 25

var make = (x: Int) -> ((y: Int) -> x + y)
println(make(5)(3))          // 8 — lambdas aninhados ✅
```

## Parâmetros

Funções suportam **parâmetros com valor padrão** (default parameters):

```kof
String saudacao(String nome = "mundo") {
    return "Olá, " + nome
}

main() {
    println(saudacao())        // Olá, mundo
    println(saudacao("Kof"))   // Olá, Kof
}
```

Não há **argumentos nomeados**. `fn`/`fun`/`func` são palavras reservadas — não existem em Kof (dão `PARSE085`). Use `String nome(...) { }` ou `nome(...): String { }`.

## Sobrecarga top-level (0.4.0+)

Funções top-level podem ter o **mesmo nome** quando as assinaturas são
distintas (tipo ou aridade) — o compilador escolhe pelo call-site, nos 4
alvos (decisão 10a):

```kof
Int dobro(Int x) { return x * 2 }
String dobro(String s) { return s + s }

Int soma(Int a, Int b) { return a + b }
Int soma(Int a, Int b, Int c) { return a + b + c }
```

Duas funções com a **mesma assinatura** são `SEM047` — ambiguidade é erro,
não sorteio.

> Cuidado com o idioma: sobrecarga é permitida, mas se duas funções fizerem
> a mesma *intenção* com tipos diferentes, prefira uma função genérica ou um
> nome que diga o domínio. O compilador deixou de proibir; seu bom senso
> continua no comando.

## Funções como valor (0.4.0+)

O tipo de função pode aparecer em parâmetro, campo e retorno — a assinatura
`(Tipo) -> Retorno`:

```kof
aplica((Int) -> Int f, Int v): Int { return f(v) }

main() {
    var op: (Int) -> Int = (x: Int) -> x + 1
    println(aplica(op, 41))     // 42
}
```

Retornar função também funciona (o tipo da lambda `-> T?` em slot `-> T` é
recusado: `SEM085`).

## Funções aninhadas (0.3.22+)

Uma função pode ser declarada **dentro** de outra — com hoisting, ou seja,
pode ser chamada antes do ponto onde aparece:

```kof
main() {
    println(aux(2))          // 102
    Int aux(Int x) { return x + 100 }
}
```

Use para lógica local curta; se precisa ser reusada fora, promova a
top-level.

## Exercícios

1. Escreva `max(a, b)` e `min(a, b)` como funções top-level.
2. Escreva `fatorial(n)` com `for`.
3. Use uma lambda para calcular `(a + b) * c` a partir de `(x: Int) -> x * c`.
4. Refatore um `class MathUtils { static ... }` para funções top-level.