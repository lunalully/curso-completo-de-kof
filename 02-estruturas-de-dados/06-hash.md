# Módulo 02 · Aula 6 — Tabela Hash e `Map`

> O `Map<K,V>` **existe** desde a 0.1.0-beta (3 targets). Este módulo ensina
> o *conceito* de hash — e mostra por que o `Map` da linguagem é o padrão.

## O conceito

Uma tabela hash mapeia **chave → valor** usando uma função hash para espalhar
as chaves em "baldes". Busca média O(1).

## A abstração da linguagem (o padrão)

```kof
main() {
    var m = mapOf()
    m.put("mel", 26)
    m.put("kof", 30)
    println(m.get("mel"))      // 26
    println(m.contains("kof")) // true
    println(m.size())          // 2
    var chaves = m.keys()      // List<String>
    for (var k in chaves) {
        println(k + " = " + m.get(k))
    }
}
```

O domínio é "chave → valor" — a intenção é o `Map`. Como a plataforma
implementa baldes é detalhe interno.

## Padrão 1: lista de entradas (para entender o custo)

Antes do `Map`, o padrão era busca linear sobre `List<record>` — útil para
*entender* o que o `Map` resolve:

```kof
record Entry(String chave, Int valor)

class Tabela {
    List<Entry> entradas

    constructor() {
        entradas = listOf()
    }

    void put(String chave, Int valor) {
        for (var i = 0; i < entradas.size; i = i + 1) {
            if (entradas.get(i).chave == chave) {
                entradas.set(i, Entry(chave, valor))
                return
            }
        }
        entradas.add(Entry(chave, valor))
    }

    Int get(String chave) {
        for (var e in entradas) {
            if (e.chave == chave) {
                return e.valor
            }
        }
        throw "chave nao existe: " + chave
    }
}
```

Busca **O(n)** — cada `get` percorre a lista inteira.

## Padrão 2: índice por balde (hash simples por primeira letra)

É assim que uma tabela hash funciona por dentro:

```kof
record Item(String chave, Int valor)

class TabelaHash {
    List<Item>[] baldes
    Int n

    constructor() {
        n = 26                     // letras a-z
        baldes = new List<Item>[26]
        for (var i = 0; i < n; i = i + 1) {
            baldes[i] = listOf()
        }
    }

    Int hash(String chave) {
        var c = chave.charAt(0)
        if (c >= 97 && c <= 122) {     // 'a'..'z'
            return c - 97
        }
        return 0
    }

    void put(String chave, Int valor) {
        var b = hash(chave)
        var lista = baldes[b]
        for (var i = 0; i < lista.size; i = i + 1) {
            if (lista.get(i).chave == chave) {
                lista.set(i, Item(chave, valor))
                return
            }
        }
        lista.add(Item(chave, valor))
    }

    Int get(String chave) {
        var lista = baldes[hash(chave)]
        for (var item in lista) {
            if (item.chave == chave) {
                return item.valor
            }
        }
        throw "chave nao existe: " + chave
    }
}
```

> A busca agora só percorre o balde da primeira letra — em média ~n/26.
> É o mesmo princípio do `Map` nativo (com função de hash melhor).

## Regra do curso

Use `mapOf()`/`setOf()` para dicionários e conjuntos reais. Os Padrões 1 e 2
existem para você entender *custo* e *mecânica* — não como substitutos do
`Map`.

> Nota: a API de `Map`/`Set` é de métodos (`size()`, `keys()`), e a forma de
> propriedade `m.size` ainda gera bytecode inválido — ver
> `00-fundamentos/99-notas-workarounds.md` (#9).

## Exercícios

1. Adicione `remove(chave)` ao `Tabela` (Padrão 1).
2. Melhore o hash do Padrão 2 para usar a soma dos caracteres mod 26.
3. Reescreva a contagem de frequência de palavras usando `Map<String, Int>`
   e compare com a versão em `Tabela`.
4. Compare (com `now()`) a busca no Padrão 1 vs Padrão 2 vs `Map.get` com
   1000 entradas.