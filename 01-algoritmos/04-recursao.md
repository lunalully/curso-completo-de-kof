# Módulo 01 · Aula 3 — Recursão

> Uma função que chama a si mesma para resolver uma versão menor do mesmo
> problema. Duas partes: **caso base** (para) e **passo recursivo** (reduz).

## Fatorial

```kof
Int fatorial(Int n) {
    if (n <= 1) {
        return 1
    }
    return n * fatorial(n - 1)
}

main() {
    println(fatorial(5))   // 120
}
```

## Fibonacci

```kof
Int fib(Int n) {
    if (n <= 1) {
        return n
    }
    return fib(n - 1) + fib(n - 2)
}
```

> **Cuidado:** Fibonacci recursivo ingênuo é O(2ⁿ). Para valores grandes,
> use iteração ou memoização (lista acumuladora).

## Iterativo é Kof (intenção primeiro)

Em Kof, a versão iterativa geralmente expressa melhor a intenção e evita
estouro de pilha:

```kof
Int fibIterativo(Int n) {
    if (n <= 1) {
        return n
    }
    var a = 0
    var b = 1
    for (var i = 2; i <= n; i = i + 1) {
        var prox = a + b
        a = b
        b = prox
    }
    return b
}
```

## Dividir e conquistar

Já vimos o merge sort (módulo 1, aula 2). Outro exemplo clássico:

```kof
Int somaVetor(List<Int> lista, Int n) {
    if (n == 0) {
        return 0
    }
    return lista.get(n - 1) + somaVetor(lista, n - 1)
}
```

## Recursão com strings — palíndromo

```kof
Bool ehPalindromo(String s) {
    if (s.length <= 1) {
        return true
    }
    if (s.charAt(0) != s.charAt(s.length - 1)) {
        return false
    }
    return ehPalindromo(s.substring(1, s.length - 1))
}

main() {
    println(ehPalindromo("radar"))    // true
    println(ehPalindromo("kof"))      // false
}
```

> **Nota por target:** `length` conta bytes (Native) ou unidades UTF-16 (JVM).
> Para acentos/emoji pode diferir — considere apenas ASCII nos exercícios.

## Quando usar recursão

- O problema é naturalmente recursivo (árvores, dividir-e-conquistar).
- Profundidade pequena (Kof não tem otimização de tail-call garantida).

## Quando NÃO usar recursão

- Quando a versão iterativa é igualmente clara (Kof prefere intenção direta).
- Profundidades grandes → use `for`/`while`.

## Exercícios

1. Implemente `potencia(base, expoente)` recursiva e iterativa.
2. Implemente `inverter(String s)` recursiva.
3. Implemente `mdc(a, b)` (algoritmo de Euclides) recursivo.
4. Conte quantas vezes um dígito aparece em um número usando recursão.