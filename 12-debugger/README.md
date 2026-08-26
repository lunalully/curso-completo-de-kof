# Trilha 12 — Debugger com Kof

> **Trilha isolada.** Pré-requisito: Trilha 00, 11. Ao terminar, você depura
> programas Kof com `kof debug` (servidor DAP) e técnicas clássicas — usando
> as ferramentas que viajam com a distribuição.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-mentalidade.md](01-mentalidade.md) | como depurar sem pânico |
| [02-cli.md](02-cli.md) | `kof debug`, DAP, breakpoints |
| [03-runtime.md](03-runtime.md) | erros de runtime e diagnósticos |
| [04-ferramentas.md](04-ferramentas.md) | `kof check`, `kof test`, logs |

## Estado real

- `kof debug <file.kf>` — servidor DAP sobre stdio (breakpoints por linha,
  call stack com funções/linhas Kof, continue, disconnect).
- `kof check` — type-check sem emitir código.
- `kof test` — blocos `test "nome" { }` com PASS/FAIL por teste.
- `kof lsp` — diagnostics no editor.
- `kof info` — ambiente.

## Como estudar

1. Leia as aulas.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

1. Escreva um programa com um bug (índice fora do intervalo).
2. `kof check` deve aceitar (runtime); `kof run` deve mostrar o erro.
3. Rode `kof debug` e inspecione o call stack.
4. Corrija e confirme com `kof test`.

Programa corrigido + teste PASS → trilha concluída.