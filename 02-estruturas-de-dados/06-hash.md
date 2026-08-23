# Módulo 02 · Aula 6 — Tabela Hash (WORKAROUND até Map)

> `Map` é **planned**, não existe. Este módulo ensina o *conceito* de hash e
> um índice prático com `List<record>` — marcado como `WORKAROUND` (não idiom).

## O conceito

Uma tabela hash mapeia **chave → valor** usando uma função hash para espalhar
as chaves em "baldes". Busca média O(1).

Em Kof, sem `Map`, o padrão realista é:

1. **`List<Entry(chave, valor)>` + busca linear** — simples, O(n).
2. **Índice por balde** — espalha por um campo calculado (ex.: primeira letra)
   para busca mais rápida.

## Padrão 1: lista de entradas (WORKAROUND)

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

    Bool contem(String chave) {
        for (var e in entradas) {
            if (e.chave == chave) {
                return true
            }
        }
        return false
    }

    Int tamanho() {
        return entradas.size
    }
}

main() {
    var t = Tabela()
    t.put("mel", 26)
    t.put("kof", 30)
    println(t.get("mel"))      // 26
    println(t.contem("kof"))   // true
}
```

## Padrão 2: índice por balde (hash simples por primeira letra)

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

## Quando isso deixa de ser WORKAROUND

Quando `Map` for implementado, o curso atualiza e o padrão vira:

```kof
var m = mapOf("mel" to 26, "kof" to 30)   // futuro
```

Enquanto isso, marque qualquer uso como `WORKAROUND` no seu código.

## Exercícios

1. Adicione `remove(chave)` e `chaves()` ao `Tabela`.
2. Melhore o hash do Padrão 2 para usar a soma dos caracteres mod 26.
3. Conte a frequência de palavras de uma string usando `Tabela` (chave = palavra).
4. Compare (com `now()`) a busca no Padrão 1 vs Padrão 2 com 1000 entradas.