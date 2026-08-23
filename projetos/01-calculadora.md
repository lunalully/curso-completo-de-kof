# Projeto 1 — Calculadora CLI com Pilha

> **Objetivo:** uma calculadora que avalia expressões pós-fixa (RPN) usando a
> pilha do módulo 02. CLI puro com `println`.

## Requisitos

- Módulos: 00 (funções, loops, strings), 02 (pilha).
- Entrada: expressão pós-fixa em linha de comando (args do programa).
- Saída: o resultado, ou um erro claro.

## Como funciona RPN

```
"3 4 +"     → 7
"5 1 2 + 4 * + 3 -"   → 14
```

Empilhe números; ao encontrar um operador (`+ - * /`), desempilhe os dois
topos, calcule e empilhe o resultado.

## Passo a passo

1. Crie uma `PilhaDeDouble` (especialize a pilha do módulo 02).
2. Leia os tokens com `split(" ")`.
3. Para cada token: se é número, empilhe; se é operador, aplique.
4. No final, desempilhe o resultado.
5. Trate erros: divisão por zero, expressão inválida, pilha vazia.

## Esqueleto

```kof
class PilhaDeDouble {
    List<Double> itens
    constructor() {
        itens = listOf()
    }
    void empilhar(Double v) {
        itens.add(v)
    }
    Double desempilhar() {
        if (itens.size == 0) {
            throw "pilha vazia"
        }
        var topo = itens.get(itens.size - 1)
        itens.remove(itens.size - 1)
        return topo
    }
    Bool vazia() {
        return itens.size == 0
    }
}

Double avaliar(String expressao) {
    var pilha = PilhaDeDouble()
    var tokens = expressao.split(" ")
    for (var i = 0; i < tokens.length; i = i + 1) {
        var t = tokens[i]
        // detecte número vs operador e aplique
    }
    return pilha.desempilhar()
}

main() {
    println(avaliar("3 4 +"))
    println(avaliar("5 1 2 + 4 * + 3 -"))
}
```

> **Nota:** para converter `String` → `Double`, verifique o que seu
> compilador oferece (pode não existir `toDouble`). Se não houver, use
> números inteiros com `toInt` próprio ou mantenha `Int`.

## Critérios de aceite

- [ ] `3 4 +` → `7`
- [ ] `5 1 2 + 4 * + 3 -` → `14`
- [ ] Divisão por zero lança erro claro
- [ ] Expressão inválida não crasha (erro tratado)
- [ ] Código idiomático (sem classes utilitárias)

## Extensões

- Suporte a unário (`-3`).
- Suporte a `sqrt`, `pow`.
- Converter notação **infixa → pós-fixa** (shunting-yard).