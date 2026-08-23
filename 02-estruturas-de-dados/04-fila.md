# Módulo 02 · Aula 4 — Fila (FIFO)

> Primeiro a entrar, primeiro a sair. Filas modelam *ordem de chegada*:
> tarefas, mensagens, impressão, atendimento.

```kof
class Fila {
    List<Int> itens

    constructor() {
        itens = listOf()
    }

    void enfileirar(Int valor) {
        itens.add(valor)
    }

    Int desenfileirar() {
        if (itens.size == 0) {
            throw "fila vazia"
        }
        var primeiro = itens.get(0)
        itens.remove(0)
        return primeiro
    }

    Int frente() {
        if (itens.size == 0) {
            throw "fila vazia"
        }
        return itens.get(0)
    }

    Bool vazia() {
        return itens.size == 0
    }

    Int tamanho() {
        return itens.size
    }
}
```

> **Nota de implementação:** `remove(0)` em um `ArrayList` custa O(n) (shift).
> Para filas de alto volume, use um índice circular sobre array. Para uso
> geral, a versão com `List` é clara e suficiente — *otimize só o ponto
> medido*.

## Fila circular sobre array (O(1) enfileirar/desenfileirar)

```kof
class FilaCircular {
    Int[] buffer
    Int inicio
    Int fim
    Int n

    constructor(Int capacidade) {
        buffer = new Int[capacidade]
        inicio = 0
        fim = 0
        n = 0
    }

    Bool cheia() {
        return n == buffer.length
    }

    Bool vazia() {
        return n == 0
    }

    void enfileirar(Int valor) {
        if (cheia()) {
            throw "fila cheia"
        }
        buffer[fim] = valor
        fim = (fim + 1) % buffer.length
        n = n + 1
    }

    Int desenfileirar() {
        if (vazia()) {
            throw "fila vazia"
        }
        var valor = buffer[inicio]
        inicio = (inicio + 1) % buffer.length
        n = n - 1
        return valor
    }
}

main() {
    var f = FilaCircular(4)
    f.enfileirar(1)
    f.enfileirar(2)
    f.enfileirar(3)
    println(f.desenfileirar())   // 1
    println(f.desenfileirar())   // 2
    f.enfileirar(4)
    println(f.desenfileirar())   // 3
    println(f.desenfileirar())   // 4
}
```

## BFS (busca em largura) usa fila

```kof
// Veja também a aula de grafos (05-grafos.md). Aqui o conceito:
// processa-se nível por nível usando uma fila de "visitar".
```

## Aplicações reais

- **Filas de trabalho** (worker pools).
- **BFS** em grafos e árvores.
- **Buffers de eventos/mensagens** (o `kof.messaging` planejado).
- Escalonamento justo (round-robin).

## Exercícios

1. Implemente `FilaString` para processar mensagens na ordem de chegada.
2. Use `FilaCircular` para um buffer de teclas: enfileire 5, desenfileire 3,
   enfileire 2 e confirme a ordem.
3. Implemente BFS em um grafo (próxima aula) usando `Fila`.
4. Compare o custo de `remove(0)` vs fila circular com `now()` em 100k ops.