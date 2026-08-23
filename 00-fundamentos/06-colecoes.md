# Módulo 00 · Aula 6 — Coleções

> `List<T>` é a coleção ordenada da linguagem. `Map`/`Set` ainda não existem.

## Criar e usar uma lista

```kof
var l = listOf(1, 2, 3, 4)
l.add(5)              // também: push / append
var x = l.get(0)      // também: l[0]
l.set(0, 9)
l.size                // ou l.length / l.count / l.size()
l.contains(3)
l.isEmpty()
var removido = l.remove(1)
l.clear()
var vazio = listOf<Int>()
```

Disponível em JVM (ArrayList) e Native (implementação própria) com a mesma API.

## Tipos de elementos

```kof
var ids = listOf<Int>()            // lista vazia de Int
var nomes = listOf("Ana", "Mel")
var users = listOf<User>()         // lista de objetos (erasure)
```

## Iteração

```kof
for (var item in listOf("a", "b", "c")) {
    println(item)
}
```

`for-in` funciona sobre `List<T>` **e** arrays (`new Int[5]`).

## Quando NÃO usar coleção manual

```kof
// RUIM — estrutura manual (implementação acidental)
class Node {
    Node next
    Int value
}
class Registry {
    Node root
    Int count
}

// BOM — a abstração da linguagem
class Registry {
    List<LanguageEntry> entries
    constructor() {
        entries = listOf(LanguageEntry("Kof", "kf"))
    }
}
```

O domínio é "uma sequência de entradas" — represente o domínio, não nós
encadeados.

## `Map` e `Set` (planned — não use)

`Map` e `Set` **não existem ainda**. Não finja que existem:

```kof
// NÃO COMPILA
var m = HashMap()
var s = setOf(1, 2, 3)
```

Para associações nome-valor, use `List<record>` com busca linear:

```kof
record Entry(String key, String value)

String buscar(List<Entry> entries, String key) {
    for (var e in entries) {
        if (e.key == key) {
            return e.value
        }
    }
    throw "chave nao encontrada: " + key
}
```

## Array como substituto de coleção dinâmica

Use `List<T>` para sequências que crescem. Arrays (`new Int[n]`) são para
tamanho fixo e acesso indexado.

## Exercícios

1. Some os elementos de um `listOf(1..100)`.
2. Crie uma função `contem(list, alvo)` que retorna `Bool`.
3. Conte quantas vezes cada letra aparece em uma string usando
   `List<record Letra(Char c, Int n)>`.
4. Implemente `ordenar(list)` (bubble sort) — será usado no módulo 1.