# Trilha 01 — Algoritmos em Kof

> **Trilha isolada.** Pré-requisito: [Trilha 00](../00-fundamentos/README.md).
> Ao terminar, você implementa algoritmos clássicos em Kof idiomático e
> analisa complexidade.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [00-intro.md](00-intro.md) | como pensar algoritmicamente em Kof |
| [02-busca.md](02-busca.md) | busca linear e binária |
| [03-ordenacao.md](03-ordenacao.md) | bubble, selection, insertion, merge |
| [04-recursao.md](04-recursao.md) | recursão, dividir e conquistar |
| [05-complexidade.md](05-complexidade.md) | Big-O e medição com `now()` |

## Como estudar

1. Leia as aulas na ordem e rode os exemplos.
2. Faça os [exercícios](exercicios.md) por nível.
3. Confira as [soluções](solucoes/) depois de tentar.
4. Passe no checkpoint.

## Checkpoint

Em um único arquivo, implemente e teste:
1. `buscaBinaria(List<Int>, alvo)` (ordenada).
2. `mergeSort(List<Int>)`.
3. `fib(Int)` recursivo E iterativo.
4. Uma função O(n²) e uma O(n), e meça a diferença com `now()` para n=5000.

Rode `kof check` e `kof run` sem erros → trilha concluída.