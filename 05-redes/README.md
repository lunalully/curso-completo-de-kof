# Trilha 05 — Redes em Kof

> **Trilha isolada.** Pré-requisito: Trilha 00. Ao terminar, você entende a
> pilha TCP/IP, o HTTP e a concorrência — e sabe o que a plataforma abstrai.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-modelo.md](01-modelo.md) | modelo TCP/IP, endereçamento |
| [02-http.md](02-http.md) | HTTP: request/response, métodos, status |
| [03-sockets.md](03-sockets.md) | sockets (o que a plataforma abstrai) |
| [04-concorrencia.md](04-concorrencia.md) | `spawn` por intenção |

## Como estudar

1. Leia as aulas. Os conceitos de rede são teóricos — **explique com suas
   palavras** nos exercícios.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

1. Explique (escrito) o caminho de um GET desde o browser até a rota Kof.
2. Rode um servidor `web.app()` e teste com `curl` (método, path, query).
3. Escreva um programa com 3 `spawn` e confirme o join implícito.

`kof run` sem erros → trilha concluída.