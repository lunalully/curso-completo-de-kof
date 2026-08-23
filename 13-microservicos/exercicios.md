# Trilha 13 · Exercícios

> ★ conceitual · ★★ prático · ★★★ aplicado.

## Conceitos (aula 01)

- ★ Explique microserviços com suas palavras; liste 3 sinais de que **NÃO** usar (time pequeno, domínio simples, dados fortemente acoplados).
- ★★ Desenhe (em texto) o fluxo: `cliente → gateway → pedidos → estoque`.

## Serviço Kof (aula 02)

- ★★ [01-servico-pedidos.kf](solucoes/01-servico-pedidos.kf) — serviço de pedidos: POST cria, GET lista, `/health`.
- ★★ [02-servico-estoque.kf](solucoes/02-servico-estoque.kf) — serviço de estoque: consulta e baixa de quantidade, `/health`.

## Comunicação (aula 03)

- ★★★ [03-comunicacao.md](solucoes/03-comunicacao.md) — escreva o contrato HTTP entre os dois serviços (request/response) e o `WORKAROUND` do cliente HTTP.
- ★★★ [04-gateway.kf](solucoes/04-gateway.kf) — serviço gateway que roteia `/pedidos` e `/estoque` (no seu serviço; note que a chamada ao backend é o gap).

## Config e operação (aula 05)

- ★★ [05-config-por-servico.md](solucoes/05-config-por-servico.md) — defina `kof.config` para cada serviço (nome, porta) e como executar os dois juntos.

## Desafio integrador

[06-integrador.kf](solucoes/06-integrador.kf) — **sistema de pedidos**:
1. Serviço `pedidos` (porta 8081): criar pedido, listar, `/health`.
2. Serviço `estoque` (porta 8082): consultar produto, dar baixa, `/health`.
3. Ambos com `kof.config` + `kof.log`.
4. Documente: contrato entre serviços + rota de composição + gap do cliente HTTP.

Rode os dois serviços e teste cada rota → trilha concluída.