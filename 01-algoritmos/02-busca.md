# Módulo 01 · Aula 1 — Busca

## Busca linear — O(n)

A mais simples: percorre a lista até achar.

```kof
Int buscarIndice(List<Int> lista, Int alvo) {
    for (var i = 0; i < lista.size; i = i + 1) {
        if (lista.get(i) == alvo) {
            return i
        }
    }
    throw "nao encontrado: " + alvo
}

Bool contem(List<Int> lista, Int alvo) {
    for (var x in lista) {
        if (x == alvo) {
            return true
        }
    }
    return false
}
```

> **Idioma:** quando a intenção é só *existência*, use o laço com retorno
> direto. Não invente um `Optional` — ou retorne `Bool`, ou lance exceção
> (veja a aula de erros).

### Busca linear em records

```kof
record User(Int id, String name)

User buscarPorId(List<User> users, Int id) {
    for (var u in users) {
        if (u.id == id) {
            return u
        }
    }
    throw "user nao encontrado: " + id
}
```

## Busca binária — O(log n)

Requer a lista **ordenada**. Divide o espaço pela metade a cada passo.

```kof
// assume que lista está ordenada (crescente)
Int buscaBinaria(List<Int> lista, Int alvo) {
    var baixo = 0
    var alto = lista.size - 1
    while (baixo <= alto) {
        var meio = (baixo + alto) / 2
        var valor = lista.get(meio)
        if (valor == alvo) {
            return meio
        }
        if (valor < alvo) {
            baixo = meio + 1
        } else {
            alto = meio - 1
        }
    }
    throw "nao encontrado: " + alvo
}
```

### Busca binária recursiva

```kof
Int buscaBinariaRec(List<Int> lista, Int alvo, Int baixo, Int alto) {
    if (baixo > alto) {
        throw "nao encontrado: " + alvo
    }
    var meio = (baixo + alto) / 2
    var valor = lista.get(meio)
    if (valor == alvo) {
        return meio
    }
    if (valor < alvo) {
        return buscaBinariaRec(lista, alvo, meio + 1, alto)
    }
    return buscaBinariaRec(lista, alvo, baixo, meio - 1)
}
```

## main() de verificação

```kof
main() {
    var dados = listOf(3, 7, 11, 19, 42, 99)
    println(buscaBinaria(dados, 42))          // 4
    println(buscaBinariaRec(dados, 7, 0, dados.size - 1))  // 1
    println(contem(dados, 100))               // false
}
```

## Comparação

| Algoritmo | Melhor | Pior | Requisito |
|-----------|--------|------|-----------|
| Busca linear | O(1) | O(n) | nenhum |
| Busca binária | O(1) | O(log n) | lista ordenada |

## Exercícios

1. Implemente `ultimoIndice(lista, alvo)` — o último índice onde o alvo aparece.
2. Implemente `contar(lista, alvo)` — quantas vezes o alvo aparece.
3. Implemente busca binária genérica para `List<String>` (comparação com `==`
   e `<` — strings são comparáveis por conteúdo em Kof).
4. Qual é a vantagem prática da busca binária sobre a linear? Quando a linear
   ainda é a escolha certa?