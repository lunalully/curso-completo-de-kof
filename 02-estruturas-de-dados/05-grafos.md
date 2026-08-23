# Módulo 02 · Aula 5 — Grafos

> Grafos modelam **relações**: redes sociais, rotas, dependências, internet.
> Em Kof representamos vértices como records e adjacências com `List<T>`.

## Representação: lista de adjacência

```kof
record Vertice(String nome)

class Grafo {
    List<Vertice> vertices
    List<Vertice>[] adj   // adj[i] = vizinhos do vértice i
    Int tamanho

    constructor() {
        vertices = listOf()
        adj = new List<Vertice>[0]  // WORKAROUND: arrays não crescem
        tamanho = 0
    }
}
```

> **Nota real:** arrays de `List` crescem mal em Kof hoje. O padrão prático e
> claro é **grafo como lista de arestas** (edges) ou **mapa vértice → vizinhos**
> usando `List<record>`. Veja o modelo abaixo.

## Modelo idiomático com records (grafo não-dirigido)

```kof
record Aresta(String de, String para)

class Grafo {
    List<String> vertices
    List<Aresta> arestas

    constructor() {
        vertices = listOf()
        arestas = listOf()
    }

    void adicionarVertice(String nome) {
        if (!vertices.contains(nome)) {
            vertices.add(nome)
        }
    }

    void adicionarAresta(String de, String para) {
        adicionarVertice(de)
        adicionarVertice(para)
        arestas.add(Aresta(de, para))
        arestas.add(Aresta(para, de))   // não-dirigido
    }

    Bool saoVizinhos(String a, String b) {
        for (var e in arestas) {
            if (e.de == a && e.para == b) {
                return true
            }
        }
        return false
    }

    List<String> vizinhos(String v) {
        var lista = listOf<String>()
        for (var e in arestas) {
            if (e.de == v) {
                lista.add(e.para)
            }
        }
        return lista
    }
}
```

## DFS (profundidade) — com pilha

```kof
List<String> dfs(Grafo g, String inicio) {
    var visitados = listOf<String>()
    var pilha = listOf(inicio)
    while (pilha.size > 0) {
        var v = pilha.get(pilha.size - 1)
        pilha.remove(pilha.size - 1)
        if (visitados.contains(v)) {
            continue
        }
        visitados.add(v)
        for (var viz in g.vizinhos(v)) {
            if (!visitados.contains(viz)) {
                pilha.add(viz)
            }
        }
    }
    return visitados
}
```

## BFS (largura) — com fila

```kof
List<String> bfs(Grafo g, String inicio) {
    var visitados = listOf<String>()
    var fila = listOf(inicio)
    while (fila.size > 0) {
        var v = fila.get(0)
        fila.remove(0)
        if (visitados.contains(v)) {
            continue
        }
        visitados.add(v)
        for (var viz in g.vizinhos(v)) {
            if (!visitados.contains(viz)) {
                fila.add(viz)
            }
        }
    }
    return visitados
}
```

## main() de verificação

```kof
main() {
    var g = Grafo()
    g.adicionarAresta("A", "B")
    g.adicionarAresta("A", "C")
    g.adicionarAresta("B", "D")
    g.adicionarAresta("C", "D")

    println(g.saoVizinhos("A", "B"))   // true
    println(dfs(g, "A"))               // ordem de profundidade
    println(bfs(g, "A"))               // ordem por nível
}
```

## Exercícios

1. Adicione `conectados(a, b)` que devolve `true` se existe caminho (use DFS).
2. Conte o número de **componentes conexas** do grafo.
3. Implemente grafo **dirigido** (sem duplicar a aresta reversa).
4. Detecte ciclo: se em um DFS você encontra um vértice já visitado que não é
   o pai imediato, há ciclo.