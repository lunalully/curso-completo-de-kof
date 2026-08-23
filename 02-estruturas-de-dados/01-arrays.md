# Módulo 02 · Aula 1 — Arrays

> Arrays são **tamanho fixo** e de **acesso indexado O(1)**. Não crescem.

## Criação

```kof
var arr = new Int[10]       // 10 inteiros, todos 0
var strings = new String[5] // 5 slots de String
var vazio = new Int[0]      // array vazio
```

## Acesso e atribuição

```kof
arr[0] = 10
println(arr[0])      // 10
println(arr.length)  // 10
```

Acesso fora do intervalo gera erro em runtime
(`array index out of bounds`).

## Como parâmetro e retorno

```kof
Int somar(Int[] arr) {
    var total = 0
    for (var i = 0; i < arr.length; i = i + 1) {
        total = total + arr[i]
    }
    return total
}

Int[] quadrados(Int n) {
    var a = new Int[n]
    for (var i = 0; i < n; i = i + 1) {
        a[i] = i * i
    }
    return a
}

main() {
    var q = quadrados(5)
    println(q[4])            // 16
    println(somar(q))        // 30
}
```

## for-in sobre arrays

```kof
var nums = new Int[3]
nums[0] = 5
for (var n in nums) {
    println(n)
}
```

## Array vs List

| | Array | List |
|--|-------|------|
| Tamanho | fixo | dinâmico |
| Crescimento | não | sim (`add`) |
| Acesso | O(1) | O(1) |
| Iteração | `for-in` / índice | `for-in` / índice |
| Uso típico | buffers, matrizes, fixos | coleções que crescem |

**Regra:** sequência que cresce → `List<T>`. Tamanho fixo e acesso indexado →
array. Não use array como substituto de coleção dinâmica.

## Matriz (array de arrays)

Kof não tem array 2D direto, mas cada elemento pode ser um array:

```kof
var linhas = new Int[3]
linhas[0] = new Int[2]     // ... (preencha com cuidado)
```

Prefira `List<List<Int>>` para matrizes dinâmicas.

## Exercícios

1. Inverta um array de `Int` in-place (troque extremidades).
2. Encontre o maior e o menor elemento de um array em uma passada.
3. Conte quantos elementos são pares.
4. Copie um array para outro e verifique que são independentes.