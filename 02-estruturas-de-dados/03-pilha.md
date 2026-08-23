# Módulo 02 · Aula 3 — Pilha (LIFO)

> Último a entrar, primeiro a sair. Pilhas são onipresentes: chamadas de
> função, desfazer/refazer, expressões, backtracking.

Em Kof, uma pilha é uma classe que **encapsula** um `List<T>` e expõe só as
operações de pilha — escondendo a representação interna.

```kof
class Pilha {
    List<Int> itens

    constructor() {
        itens = listOf()
    }

    void empilhar(Int valor) {
        itens.add(valor)
    }

    Int desempilhar() {
        if (itens.size == 0) {
            throw "pilha vazia"
        }
        var topo = itens.get(itens.size - 1)
        itens.remove(itens.size - 1)
        return topo
    }

    Int topo() {
        if (itens.size == 0) {
            throw "pilha vazia"
        }
        return itens.get(itens.size - 1)
    }

    Bool vazia() {
        return itens.size == 0
    }

    Int tamanho() {
        return itens.size
    }
}
```

## Exemplo de uso: desfazer (undo)

```kof
record Acao(String descricao)

class Historico {
    Pilha pilha = Pilha()
    // Kof não tem Pilha<P> genérica com erasure — use Pilha de Int com ids,
    // ou especialize. Exemplo didático abaixo.
}
```

> **Nota:** com erasure de generics, o padrão prático é especializar a pilha
> para o tipo do domínio (ex.: `PilhaDeAcoes`). O corpo é idêntico.

## Exemplo concreto: balanceamento de parênteses

```kof
class PilhaChar {
    List<Char> itens
    constructor() {
        itens = listOf()
    }
    void empilhar(Char c) {
        itens.add(c)
    }
    Char desempilhar() {
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

Bool balanceado(String s) {
    var pilha = PilhaChar()
    for (var i = 0; i < s.length; i = i + 1) {
        var c = s.charAt(i)
        if (c == 40) {                    // '('
            pilha.empilhar(c)
        } else if (c == 41) {             // ')'
            if (pilha.vazia()) {
                return false
            }
            pilha.desempilhar()
        }
    }
    return pilha.vazia()
}

main() {
    println(balanceado("((a+b)*(c))"))   // true
    println(balanceado("((a+b)"))        // false
}
```

## Aplicações reais

- **Desfazer/refazer** em editores (veja o Kof Editor!).
- Avaliação de expressões (pós-fixa, shunting-yard).
- Percurso de árvores e DFS.
- Funções recursivas (a pilha de chamadas).

## Exercícios

1. Implemente `PilhaString` e use para inverter uma frase palavra por palavra.
2. Use a pilha para converter decimal → binário (divida por 2, empilhe restos,
   desempilhe para imprimir).
3. Valide parênteses, colchetes `[` (91) e chaves `{` (123) com correspondência.
4. Implemente DFS em um grafo usando pilha (veja a aula de grafos).