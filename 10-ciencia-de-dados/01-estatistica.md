# Trilha 10 · Aula 1 — Estatística Descritiva

> Média, mediana, moda, variância e desvio padrão — implementados em Kof puro.

## Média

```kof
Double media(List<Double> dados) {
    if (dados.size == 0) {
        throw "conjunto vazio"
    }
    var soma = 0.0
    for (var x in dados) {
        soma = soma + x
    }
    return soma / dados.size
}
```

## Mediana

Requer **ordenação**. Adapte o merge sort do módulo 01 para `Double`:

```kof
Double mediana(List<Double> dados) {
    if (dados.size == 0) {
        throw "conjunto vazio"
    }
    var ord = mergeSort(dados)
    var meio = ord.size / 2
    if (ord.size % 2 == 1) {
        return ord.get(meio)
    }
    return (ord.get(meio - 1) + ord.get(meio)) / 2.0
}
```

## Variância e desvio padrão

```kof
Double variancia(List<Double> dados) {
    var m = media(dados)
    var soma = 0.0
    for (var x in dados) {
        soma = soma + (x - m) * (x - m)
    }
    return soma / dados.size
}

Double desvioPadrao(List<Double> dados) {
    // raiz quadrada — verifique no seu compilador a função; se não houver,
    // implemente `sqrt` com o método de Newton (exercício).
    return raizQuadrada(variancia(dados))
}
```

## Moda

Frequência com `List<Entry(valor, contagem)>` (padrão do módulo 02):

```kof
record Contagem(Double valor, Int n)
```

## Exercício integrado

Rode com `listOf(1.0, 2.0, 2.0, 3.0, 4.0, 7.0, 9.0)`:
- média ≈ 4.0 · mediana = 3.0 · moda = 2.0 · desvio padrão ≈ 2.62.