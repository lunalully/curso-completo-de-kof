# Trilha 13 — Microserviços com Kof

> **Trilha isolada.** Pré-requisito: Trilhas 03, 06, 08, 09. Ao terminar, você
> projeta e implementa sistemas distribuídos com `kof.web` — seguindo a
> filosofia: *cada serviço é um programa Kof; a comunicação é HTTP; o resto é
> da plataforma*.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-aulas.md](01-aulas.md) | conceitos, serviço Kof, comunicação, gateway, config/ops |

## Estado real (honesto)

| Capacidade | Estado |
|------------|--------|
| Servidor HTTP (`web.app()`) | ✅ JVM |
| `kof.config` (per-serviço via env/arquivo) | ✅ JVM |
| `kof.log` (per-serviço) | ✅ JVM |
| `spawn` (paralelismo dentro do serviço) | ✅ JVM |
| **Cliente HTTP** (`kof.http` client) | ⏳ **planned** — `WORKAROUND` para chamar outro serviço |
| Service discovery, load balancer | ⏳ planned (config manual / gateway) |

**O gap mais importante:** hoje o Kof **não tem cliente HTTP** para *chamar*
outros serviços. A trilha ensina o desenho correto e documenta o `WORKAROUND`
(proxy/CLI externo até `kof.http` existir).

## Como estudar

1. Leia as aulas na ordem.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

1. Implemente dois serviços Kof (`pedidos` na 8081, `estoque` na 8082).
2. Cada um com rota de health (`/health`) e uma rota de negócio.
3. `kof.config` para porta e nome.
4. Documente o fluxo de composição (gateway) e o `WORKAROUND` do cliente HTTP.

Serviços rodando e respondendo → trilha concluída.