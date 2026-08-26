# Trilha 11 — Testes Unitários com Kof

> **Trilha isolada.** Pré-requisito: Trilha 00, 01, 08. Ao terminar, você
> escreve testes unitários com `assert` + `kof test` — sem framework externo,
> com a filosofia Kof: *testar é parte da plataforma*.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-fundamentos.md](01-fundamentos.md) | `assert`, blocos `test "nome" { }`, `kof test` |
| [02-casos.md](02-casos.md) | casos de borda, valores limite, testes de exceção |
| [03-organizacao.md](03-organizacao.md) | organização de suítes por arquivo/pasta |
| [04-tdd.md](04-tdd.md) | TDD (Red-Green-Refactor) em Kof |

## Como estudar

1. Leia as aulas.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

1. Escreva 3 arquivos de teste (`testes/`) para: busca binária, pilha e a classe `Carrinho` (módulo 00).
2. Inclua casos de borda (lista vazia, valor no limite, exceções).
3. Rode `kof test testes/` — tudo PASS.

`kof test` sem falhas → trilha concluída.