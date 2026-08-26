# Trilha 02 — Estruturas de Dados em Kof

> **Trilha isolada.** Pré-requisito: Trilha 00 e 01. Ao terminar, você
> escolhe e implementa a estrutura certa para cada problema — usando o que a
> linguagem oferece (`Array`, `List<T>`, `Map<K,V>`, `Set<T>` na 0.1.0-beta).

## O que você vai dominar

| Aula | Tema |
|------|------|
| [00-intro.md](00-intro.md) | escolha da estrutura certa |
| [01-arrays.md](01-arrays.md) | arrays (tamanho fixo, índice) |
| [02-lista.md](02-lista.md) | `List<T>` (dinâmica) |
| [03-pilha.md](03-pilha.md) | pilha (LIFO) |
| [04-fila.md](04-fila.md) | fila (FIFO) e fila circular |
| [05-grafos.md](05-grafos.md) | grafos com records + List |
| [06-hash.md](06-hash.md) | tabela hash (WORKAROUND até Map) |

## Como estudar

1. Leia as aulas na ordem.
2. Faça os [exercícios](exercicios.md) por nível.
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

Implemente uma **agenda de contatos** com:
1. busca por nome (linear),
2. pilha de "desfazer" (LIFO de ações),
3. fila de "notificações" (FIFO),
4. índice hash por letra inicial (`List<Entry>` por balde).

Tudo em um arquivo, `kof check` + `kof run` sem erros → concluída.