# Módulo 02 — Estruturas de Dados

> **Objetivo:** conhecer as estruturas que a linguagem oferece **hoje**
> (`Array`, `List<T>`) e implementar as clássicas (pilha, fila, grafo, hash)
> sobre elas — com a filosofia Kof.

## Princípio central

> Represente o domínio, não a implementação acidental.

Se o problema é "uma sequência", use `List<T>` — não reimplemente nós
encadeados. Implemente uma estrutura manual **somente** quando a stdlib não
cobre o que o domínio exige (pilha, fila, grafo, tabela hash são bons
exemplos de aprendizado e de necessidade real).

## O que a linguagem oferece (estado real)

| Estrutura | Estado | Uso |
|-----------|--------|-----|
| `Array` (`new Int[n]`) | ✅ | tamanho fixo, acesso indexado, sem crescimento |
| `List<T>` + `listOf` | ✅ | sequência dinâmica (JVM: ArrayList) |
| `Map` | ⏳ planned | **não use** — associe com `List<record>` |
| `Set` | ⏳ planned | **não use** — dedupe com `List` + `contains` |
| `Option<T>` / null safety | ⏳ planned | use exceção ou `WORKAROUND` documentado |

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-arrays.md](01-arrays.md) | Arrays: criação, acesso, uso correto |
| [02-lista.md](02-lista.md) | `List<T>`: a coleção dinâmica da linguagem |
| [03-pilha.md](03-pilha.md) | Pilha (LIFO) sobre `List<T>` |
| [04-fila.md](04-fila.md) | Fila (FIFO) sobre `List<T>` |
| [05-grafos.md](05-grafos.md) | Grafos com records e `List<T>` |
| [06-hash.md](06-hash.md) | Tabela hash com `List<record>` (WORKAROUND até Map) |

## Exercício integrador (ao final do módulo)

Implemente uma **agenda de contatos** com: busca por nome (linear), busca por
índice (array), pilha de "desfazer", fila de "notificações" e um índice hash
por letra inicial — tudo com o que foi ensinado.