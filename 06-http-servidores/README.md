# Trilha 06 — HTTP e Servidores

> **Trilha isolada.** Pré-requisito: Trilha 00. Ao terminar, você constrói
> APIs REST completas com `web.app()` — sem Spring, sem container.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-web-app.md](01-web-app.md) | rotas, contexto de request, JSON |
| [02-middleware.md](02-middleware.md) | `app.use`, auth, logging |
| [03-rest.md](03-rest.md) | REST: recursos, verbos, JSON |
| [04-projeto-completo.md](04-projeto-completo.md) | db + security + config + log |

## Como estudar

1. Leia as aulas na ordem.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

Construa uma API REST de `tarefas(id, titulo, concluida)`:
1. GET lista, POST cria, PUT marca concluída, DELETE remove.
2. Middleware que loga método + path.
3. Validação: título vazio → erro no corpo.
4. `kof.config` para a porta.

Rode com `kof serve` e teste com `curl` → trilha concluída.