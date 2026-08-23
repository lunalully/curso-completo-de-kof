# Trilha 06 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado. Teste tudo com `curl`.

## web.app (aula 01)

- ★ [01-hello-rota.kf](solucoes/01-hello-rota.kf) — `/hello` e `/` com respostas.
- ★★ [02-metodos.kf](solucoes/02-metodos.kf) — rotas para os 6 métodos HTTP.
- ★★ [03-parametros.kf](solucoes/03-parametros.kf) — `/users/:id`, `/search?q=`, header, body.
- ★★★ [04-json-roundtrip.kf](solucoes/04-json-roundtrip.kf) — POST recebe record JSON, devolve codificado.

## Middleware (aula 02)

- ★★ [05-middleware-log.kf](solucoes/05-middleware-log.kf) — loga método + path em `app.use`.
- ★★★ [06-middleware-auth.kf](solucoes/06-middleware-auth.kf) — header `x-auth` protege rotas; rota pública passa.
- ★★★ [07-api-key.kf](solucoes/07-api-key.kf) — rota `/privado` exige `x-api-key` via `secrets.get`.

## REST (aula 03)

- ★★★ [08-rest-crud.kf](solucoes/08-rest-crud.kf) — CRUD de `produtos` em memória (record + `List<Produto>`).

## Projeto completo (aula 04)

- ★★★ [09-crud-db.kf](solucoes/09-crud-db.kf) — CRUD com `kof.db` + `kof.config` (requer driver JDBC).
- ★★★ [10-crud-auth-db.kf](solucoes/10-crud-auth-db.kf) — CRUD com auth (middleware `auth.authenticated()`) + db + config.

## Desafio integrador

[11-integrador.kf](solucoes/11-integrador.kf) — API de **tarefas** (checkpoint):
GET `/tarefas`, POST `/tarefas`, PUT `/tarefas/:id` (concluir), DELETE `/tarefas/:id`,
middleware de log, validação de título vazio, porta via `kof.config`.
Rode com `kof serve` e teste o fluxo completo com `curl`.