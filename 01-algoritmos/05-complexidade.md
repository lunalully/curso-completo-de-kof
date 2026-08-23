# Módulo 01 · Aula 4 — Complexidade (Big-O)

> Big-O descreve **como o tempo (ou memória) cresce** com o tamanho da
> entrada `n`. Não é sobre velocidade absoluta — é sobre *escala*.

## As classes mais comuns

| Notação | Nome | Exemplo | n=1000 |
|---------|------|---------|--------|
| O(1) | constante | acesso a array | 1 passo |
| O(log n) | logarítmica | busca binária | ~10 passos |
| O(n) | linear | busca linear, um `for` | 1000 passos |
| O(n log n) | linearítmica | merge sort | ~10.000 passos |
| O(n²) | quadrática | bubble sort, loops aninhados | 1.000.000 passos |
| O(2ⁿ) | exponencial | fibonacci ingênuo | astronômico |

## Como contar

Conte as operações dominantes (geralmente o laço mais interno):

```kof
// O(n) — um laço
Int soma(List<Int> l) {
    var t = 0
    for (var x in l) {
        t = t + x
    }
    return t
}

// O(n²) — laço dentro de laço
Int paresInversos(List<Int> l) {
    var c = 0
    for (var i = 0; i < l.size; i = i + 1) {
        for (var j = i + 1; j < l.size; j = j + 1) {
            c = c + 1
        }
    }
    return c
}

// O(log n) — o espaço é dividido pela metade
Int passosBinarios(List<Int> l, Int alvo) {
    var passos = 0
    var baixo = 0
    var alto = l.size - 1
    while (baixo <= alto) {
        passos = passos + 1
        var meio = (baixo + alto) / 2
        if (l.get(meio) == alvo) {
            return passos
        }
        if (l.get(meio) < alvo) {
            baixo = meio + 1
        } else {
            alto = meio - 1
        }
    }
    return passos
}
```

## Medindo na prática (filosofia Kof: meça antes de otimizar)

Use `now()` do `kof.time` para cronometrar:

```kof
main() {
    var dados = listOf<Int>()
    for (var i = 0; i < 10000; i = i + 1) {
        dados.add(10000 - i)
    }
    var inicio = now()
    var ordenado = mergeSort(dados)
    var fim = now()
    println("mergeSort de 10000: " + (fim - inicio) + " ms")
}
```

## Regras da filosofia aplicadas

1. **Escreva a versão idiomática primeiro.** Simplicidade > micro-performance.
2. **Meça** (com `now()`, `kof bench`, `kof profile`).
3. **Otimize apenas o ponto medido**, com comentário explicando por quê.

> Perfomance conhecida do Kof (0.0.x): Native sem GC (memória devolvida ao
> SO no exit; cuidado com programas de longa duração que alocam em loop);
> concatenação de string aloca nova string; no JVM tudo delega para a
> plataforma (ArrayList, String, GC).

## Exercícios

1. Classifique: `buscaBinaria`, `bubbleSort`, `fibIterativo`, `somaVetor`.
2. Prove com o código: por que merge sort é O(n log n) e bubble é O(n²)?
3. Cronometre bubbleSort vs mergeSort com 5000 elementos usando `now()`.
4. O que acontece com o tempo do fibonacci recursivo quando `n` cresce? Meça.