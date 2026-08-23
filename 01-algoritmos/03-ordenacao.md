# Módulo 01 · Aula 2 — Ordenação

> Ordenar é um dos problemas mais clássicos. Aqui implementamos os algoritmos
> didáticos e o merge sort — todos com `List<Int>` e troca de elementos.

## Operação básica: trocar

```kof
void trocar(List<Int> lista, Int i, Int j) {
    var temp = lista.get(i)
    lista.set(i, lista.get(j))
    lista.set(j, temp)
}
```

## Bubble sort — O(n²)

```kof
List<Int> bubbleSort(List<Int> lista) {
    var n = lista.size
    for (var i = 0; i < n - 1; i = i + 1) {
        for (var j = 0; j < n - 1 - i; j = j + 1) {
            if (lista.get(j) > lista.get(j + 1)) {
                trocar(lista, j, j + 1)
            }
        }
    }
    return lista
}
```

## Selection sort — O(n²)

A cada passo, encontra o menor dos restantes e coloca na posição correta.

```kof
List<Int> selectionSort(List<Int> lista) {
    var n = lista.size
    for (var i = 0; i < n - 1; i = i + 1) {
        var menor = i
        for (var j = i + 1; j < n; j = j + 1) {
            if (lista.get(j) < lista.get(menor)) {
                menor = j
            }
        }
        if (menor != i) {
            trocar(lista, i, menor)
        }
    }
    return lista
}
```

## Insertion sort — O(n²) (bom para dados quase ordenados)

```kof
List<Int> insertionSort(List<Int> lista) {
    var n = lista.size
    for (var i = 1; i < n; i = i + 1) {
        var chave = lista.get(i)
        var j = i - 1
        while (j >= 0 && lista.get(j) > chave) {
            lista.set(j + 1, lista.get(j))
            j = j - 1
        }
        lista.set(j + 1, chave)
    }
    return lista
}
```

## Merge sort — O(n log n)

Dividir e conquistar. **Importante:** usa uma lista auxiliar.

```kof
List<Int> mergeSort(List<Int> lista) {
    if (lista.size <= 1) {
        return lista
    }
    var meio = lista.size / 2
    var esq = listOf<Int>()
    var dir = listOf<Int>()
    for (var i = 0; i < meio; i = i + 1) {
        esq.add(lista.get(i))
    }
    for (var i = meio; i < lista.size; i = i + 1) {
        dir.add(lista.get(i))
    }
    return merge(mergeSort(esq), mergeSort(dir))
}

List<Int> merge(List<Int> a, List<Int> b) {
    var resultado = listOf<Int>()
    var i = 0
    var j = 0
    while (i < a.size && j < b.size) {
        if (a.get(i) < b.get(j)) {
            resultado.add(a.get(i))
            i = i + 1
        } else {
            resultado.add(b.get(j))
            j = j + 1
        }
    }
    while (i < a.size) {
        resultado.add(a.get(i))
        i = i + 1
    }
    while (j < b.size) {
        resultado.add(b.get(j))
        j = j + 1
    }
    return resultado
}
```

## main() de verificação

```kof
main() {
    var dados = listOf(42, 7, 11, 3, 19, 1, 99)
    println(bubbleSort(listOf(42, 7, 11, 3, 19, 1, 99)))
    println(selectionSort(listOf(42, 7, 11, 3, 19, 1, 99)))
    println(insertionSort(listOf(42, 7, 11, 3, 19, 1, 99)))
    println(mergeSort(dados))
}
```

> **Nota:** `println(lista)` imprime a representação da lista. Confira a saída
> de cada algoritmo no seu terminal.

## Comparação

| Algoritmo | Pior caso | Melhor caso | Estável | Uso típico |
|-----------|-----------|-------------|---------|------------|
| Bubble | O(n²) | O(n) | sim | didático |
| Selection | O(n²) | O(n²) | não | memória mínima de trocas |
| Insertion | O(n²) | O(n) | sim | dados quase ordenados |
| Merge | O(n log n) | O(n log n) | sim | dados grandes |

## Exercícios

1. Implemente `bubbleSort` que para cedo quando a lista já está ordenada.
2. Ordene `List<String>` em ordem alfabética (compare com `<`).
3. Implemente `mergeSort` em **arrays** (`new Int[n]`) em vez de `List`.
4. Ordene `List<User>` por `age` (adapte `merge` para comparar `u.age`).