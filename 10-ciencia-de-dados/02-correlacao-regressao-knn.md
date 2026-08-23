# Trilha 10 · Aula 2 a 5 — Correlação, Regressão, Normalização, k-NN

## Aula 2 — Correlação de Pearson

Mede a relação **linear** entre duas séries, de -1 a +1.

```kof
Double pearson(List<Double> x, List<Double> y) {
    if (x.size != y.size || x.size == 0) {
        throw "series devem ter o mesmo tamanho"
    }
    var mx = media(x)
    var my = media(y)
    var somaXY = 0.0
    var somaXX = 0.0
    var somaYY = 0.0
    for (var i = 0; i < x.size; i = i + 1) {
        var dx = x.get(i) - mx
        var dy = y.get(i) - my
        somaXY = somaXY + dx * dy
        somaXX = somaXX + dx * dx
        somaYY = somaYY + dy * dy
    }
    return somaXY / raizQuadrada(somaXX * somaYY)
}
```

## Aula 3 — Regressão Linear Simples

Modelo `y = a*x + b` por mínimos quadrados:

```kof
record Regressao(Double a, Double b)

Regressao ajustar(List<Double> x, List<Double> y) {
    var mx = media(x)
    var my = media(y)
    var somaXY = 0.0
    var somaXX = 0.0
    for (var i = 0; i < x.size; i = i + 1) {
        somaXY = somaXY + (x.get(i) - mx) * (y.get(i) - my)
        somaXX = somaXX + (x.get(i) - mx) * (x.get(i) - mx)
    }
    var a = somaXY / somaXX
    var b = my - a * mx
    return Regressao(a, b)
}

Double prever(Regressao r, Double x) {
    return r.a * x + r.b
}
```

## Aula 4 — Normalização

- **Min-max** → [0, 1]: `(x - min) / (max - min)`.
- **Z-score** → média 0, desvio 1: `(x - media) / desvio`.

Use quando atributos têm escalas diferentes (ex.: altura em cm e peso em kg).

## Aula 5 — k-NN (k vizinhos mais próximos)

```kof
record Ponto(Double x, Double y, String classe)

Double distancia(Ponto a, Ponto b) {
    var dx = a.x - b.x
    var dy = a.y - b.y
    return raizQuadrada(dx * dx + dy * dy)
}

String classificar(List<Ponto> treino, Ponto novo, Int k) {
    // 1. calcule a distância de `novo` até cada ponto de treino
    // 2. ordene por distância (ascendente)
    // 3. pegue os k primeiros e conte as classes
    // 4. devolva a classe mais frequente
}
```

## Implementando `sqrt` (raiz quadrada)

Se o compilador não tiver `sqrt`, use o **método de Newton**:

```kof
Double raizQuadrada(Double n) {
    if (n <= 0) {
        return 0
    }
    var x = n
    for (var i = 0; i < 100; i = i + 1) {
        x = (x + n / x) / 2.0
    }
    return x
}
```

## Exercício integrado

Com altura (cm) e peso (kg) de 10 pessoas:
1. Pearson entre altura e peso (esperado alto, > 0.8).
2. Regressão `peso = a*altura + b`; preveja peso para altura 180.
3. Normalize e classifique `tall/short` com k-NN (k=3).