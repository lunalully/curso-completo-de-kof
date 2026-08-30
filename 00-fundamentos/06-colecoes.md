# Módulo 00 · Aula 6 — Coleções

> `List<T>` é a coleção ordenada da linguagem. `Map<K,V>` e `Set<T>`
> também existem (nos 3 targets).

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

Disponível nos 3 targets (JVM: ArrayList; Native: implementação própria em
asm; JS: runtime embarcado) com a mesma API.

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

## Transformações funcionais: `map`, `filter`, `reduce` (0.2.0+)

`List<T>` expõe operações de alta ordem diretas — sem expor `Stream`:

```kof
main() {
    var nums = listOf(1, 2, 3, 4, 5)

    var dobrados = nums.map((x: Int) -> x * 2)          // [2,4,6,8,10]
    println(dobrados.get(1))                       // 4

    var pares = nums.filter((x: Int) -> x % 2 == 0)     // [2,4]
    println(pares.size())                          // 2

    var soma = nums.reduce(0, (acc: Int, x: Int) -> acc + x) // 15
    println(soma)

    // encadeando:
    var r = listOf(1, 2, 3, 4)
        .filter((x: Int) -> x > 1)
        .map((x: Int) -> x * 10)
    println(r.get(0))   // 20
}
```

Todos os três métodos rodam em JVM, Native e JS com a mesma semântica.
O tipo do elemento é preservado pela pipeline inteira.

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

## `Map<K,V>` e `Set<T>`

`Map` e `Set` fazem parte da linguagem — a API é de **métodos**:

```kof
var m = mapOf()             // Map vazio
m.put("a", 1)
println(m.get("a"))         // 1
println(m.contains("b"))    // false
println(m.size())           // sempre () — m.size quebra (ver notas)
var chaves = m.keys()       // List<String>
var valores = m.values()    // List<Int>
println(m.remove("a"))      // 1
println(m.isEmpty())        // false
m.clear()

var s = setOf(1, 2, 3)      // Set com elementos
s.add(3)                    // duplicado: não cresce
println(s.size())           // 3
println(s.contains(2))      // true
s.remove(1)
```

> `WORKAROUND`: use `m.size()` — a forma de propriedade `m.size` sobre
> `Map`/`Set` gera bytecode inválido (`99-notas-workarounds.md` #9).

Para associações nome-valor simples, agora o idiom é o `Map`. A alternativa
com `List<record>` continua válida quando a entrada já vem em formato de
lista ou quando a ordem importa:

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