# Módulo 02 — Estruturas de Dados

> **Objetivo:** conhecer as estruturas que a linguagem oferece **hoje**
> (`Array`, `List<T>`, `Map<K,V>`, `Set<T>`) e implementar as clássicas
> (pilha, fila, grafo) sobre elas — com a filosofia Kof.

## Princípio central

> Represente o domínio, não a implementação acidental.

Se o problema é "uma sequência", use `List<T>` — não reimplemente nós
encadeados. Se é "chave → valor", use `Map`. Implemente uma estrutura
manual **somente** quando a stdlib não cobre o que o domínio exige (pilha,
fila, grafo são bons exemplos de aprendizado e de necessidade real).

## O que a linguagem oferece (estado real)

| Estrutura | Estado | Uso |
|-----------|--------|-----|
| `Array` (`new Int[n]`) | ✅ | tamanho fixo, acesso indexado, sem crescimento |
| `List<T>` + `listOf` | ✅ | sequência dinâmica (JVM: ArrayList) |
| `List.map/filter/reduce` | ✅ 0.2.0 | transformações funcionais |
| `Map<K,V>` + `mapOf` | ✅ 0.1.0 | chave → valor (API de métodos: `size()`) |
| `Set<T>` + `setOf` | ✅ 0.1.0 | conjunto sem duplicatas |
| Null safety `String?`, `Int?` | ✅ 0.2.0 | tipos anuláveis com narrowing |

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-arrays.md](01-arrays.md) | Arrays: criação, acesso, uso correto |
| [02-lista.md](02-lista.md) | `List<T>`: a coleção dinâmica da linguagem |
| [03-pilha.md](03-pilha.md) | Pilha (LIFO) sobre `List<T>` |
| [04-fila.md](04-fila.md) | Fila (FIFO) sobre `List<T>` |
| [05-grafos.md](05-grafos.md) | Grafos com records e `List<T>` |
| [06-hash.md](06-hash.md) | Tabela hash: conceito, índice por balde e `Map` nativo |

## Exercício integrador (ao final do módulo)

Implemente uma **agenda de contatos** com: busca por nome (linear), busca por
índice (array), pilha de "desfazer", fila de "notificações" e um índice hash
por letra inicial — tudo com o que foi ensinado.