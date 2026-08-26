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

**Capturas (0.1.0-beta):** lambdas capturam variáveis do escopo. O caso
sólido é capturar campos em lambda **sem parâmetros**:

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
```

> `WORKAROUND` — lambda com captura **e** parâmetro tipado
> (`(x: Int) -> x + offset`) ainda falha em runtime (VerifyError). Passe o
> valor como parâmetro. Detalhes em `99-notas-workarounds.md` (#8).

## Parâmetros

Não há sobrecarga de funções top-level nem argumentos nomeados/opcionais em
funções definidas pelo usuário (verifique o que compila antes de usar). Para
valores padrão em classes, veja o exemplo do `Style` em `07-frontend`.

## Exercícios

1. Escreva `max(a, b)` e `min(a, b)` como funções top-level.
2. Escreva `fatorial(n)` com `for`.
3. Use uma lambda para calcular `(a + b) * c` a partir de `(x: Int) -> x * c`.
4. Refatore um `class MathUtils { static ... }` para funções top-level.