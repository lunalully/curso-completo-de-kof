# Trilha 05 · Exercícios

> ★ conceitual · ★★ prático · ★★★ aplicado.

## Modelo TCP/IP (aula 01)

- ★ Explique, por escrito, cada camada (Aplicação/Transporte/Internet/Acesso) com um exemplo.
- ★ Qual a diferença entre IP e porta? Dê um exemplo real.
- ★★ [01-servidor-minimo.kf](solucoes/01-servidor-minimo.kf) — servidor com uma rota; explique o que o runtime fez "por baixo" (socket/bind/listen/accept).

## HTTP (aula 02)

- ★ Escreva um request HTTP GET e um POST com headers e corpo, à mão.
- ★ Liste os 6 métodos e a intenção de cada um.
- ★★ [02-rotas.kf](solucoes/02-rotas.kf) — crie rotas GET/POST/PUT/DELETE e teste com `curl`.
- ★★ [03-contexto.kf](solucoes/03-contexto.kf) — use `param`, `query`, `header`, `body`, `method`, `path` numa rota.

## Sockets (aula 03)

- ★ Explique a sequência de chamadas que `app.listen(8080)` dispara.
- ★★ [04-listen-efemero.kf](solucoes/04-listen-efemero.kf) — use `listen(0)` + `app.port()` e imprima a porta real.

## Concorrência (aula 04)

- ★★ [05-spawn.kf](solucoes/05-spawn.kf) — 3 `spawn` de funções + join implícito.
- ★★ [06-spawn-lote.kf](solucoes/06-spawn-lote.kf) — processe 100 itens em 4 lotes paralelos.
- ★★★ [07-spawn-native.kf](solucoes/07-spawn-native.kf) — compile para native e **valide que spawn funciona** via pthread (CONC001 fechado, 0.2.8-beta).

## Desafio integrador

[08-integrador.kf](solucoes/08-integrador.kf) — servidor que:
1. loga cada request (método + path) num middleware,
2. tem rota `/status` que devolve JSON com `method` e `path`,
3. processa uma fila de 20 itens com `spawn` em 2 lotes.