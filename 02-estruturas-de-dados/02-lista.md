# Módulo 02 · Aula 2 — List\<T\>

> A coleção ordenada e dinâmica da linguagem. Quando o domínio é "uma
> sequência de elementos", a resposta é `List<T>`.

## Criação

```kof
var l = listOf(1, 2, 3, 4)
var vazio = listOf<Int>()
var nomes = listOf("Ana", "Mel")
var users = listOf<User>()
```

## API real (verificada no compilador)

```kof
var l = listOf(1, 2, 3, 4)

l.add(5)              // append (também: push)
var x = l.get(0)      // leitura (também l[0])
l.set(0, 9)           // escrita
l.size                // ou l.length / l.count / l.size()
l.contains(3)         // Bool
l.isEmpty()           // Bool
var r = l.remove(1)   // remove e devolve o elemento
l.clear()
```

## Iteração

```kof
for (var item in listOf("a", "b", "c")) {
    println(item)
}
```

`for-in` funciona sobre `List<T>` e arrays.

## Lista de records (o padrão dos próximos módulos)

```kof
record Produto(String nome, Double preco)

class Carrinho {
    List<Produto> itens
    constructor() {
        itens = listOf()
    }
    void adicionar(Produto p) {
        itens.add(p)
    }
    Int tamanho() {
        return itens.size
    }
    Double total() {
        var soma = 0.0
        for (var p in itens) {
            soma = soma + p.preco
        }
        return soma
    }
}
```

## Não existe Map — associação com List<record>

```kof
record Entry(String chave, String valor)

class Config {
    List<Entry> entradas
    constructor() {
        entradas = listOf()
    }
    void set(String chave, String valor) {
        entradas.add(Entry(chave, valor))
    }
    String get(String chave) {
        for (var e in entradas) {
            if (e.chave == chave) {
                return e.valor
            }
        }
        throw "chave nao existe: " + chave
    }
}
```

> Isto é um padrão didático — e um `WORKAROUND` até `Map` existir. Para o
> dia a dia, veja `06-hash.md` (índice por chave) e `kof.config` (configuração
> real da plataforma).

## Exercícios

1. Implemente `reverter(List<T>)` genérica o máximo que o type erasure permitir.
2. Implemente `unico(List<Int>)` que devolve a lista sem duplicatas (List + contains).
3. Implemente `frequencia(String s)` devolvendo `List<Entry(char, Int)>`.
4. Implemente `particionar(List<Int>, Bool predicate)` que devolve duas listas
   (pares e ímpares).