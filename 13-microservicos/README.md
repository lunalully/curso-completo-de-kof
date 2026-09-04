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
| **Cliente HTTP** (`kof.http`: get/post/put/delete/patch) | ✅ 3 targets (HTTP002 fechado) |
| `kof.config` (per-serviço via env/arquivo) | ✅ JVM |
| `kof.log` (per-serviço) | ✅ JVM |
| `spawn` (paralelismo dentro do serviço) | ✅ JVM |
| Service discovery, load balancer | ⏳ planned (config manual / gateway) |

**O gap restante** é descoberta de serviço: endereços vão no `kof.config`
de cada serviço (ou num gateway). A comunicação em si é `http.get/post`
nativo.

## Como estudar

1. Leia as aulas na ordem.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

1. Implemente dois serviços Kof (`pedidos` na 8081, `estoque` na 8082).
2. Cada um com rota de health (`/health`) e uma rota de negócio.
3. `kof.config` para porta e nome.
4. `pedidos` chama `estoque` com `http.get`/`http.post` e trata status.
5. Documente o fluxo de composição (gateway).

Serviços rodando e conversando → trilha concluída.