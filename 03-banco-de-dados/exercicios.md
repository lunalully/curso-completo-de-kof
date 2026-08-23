# Trilha 03 · Exercícios

> ★ básico · ★★ intermediário · ★★★ avançado. Rode com o driver JDBC no
> classpath (ver aula 02). Compile sempre com `kof check`.

## SQL (aula 01)

- ★ Escreva no papel: CREATE TABLE `clientes(id, nome, email)`, 3 INSERTs, um SELECT com `where` e `order by`.
- ★ Escreva UPDATE e DELETE para um cliente.
- ★★ Escreva uma query com `count(*)` e uma com `join` (pedidos × clientes).
- ★★★ Modele um sistema de pedidos: `clientes`, `pedidos`, `pedido_itens`.

## kof.db (aula 02)

- ★ [01-conectar.kf](solucoes/01-conectar.kf) — conecte, crie tabela, feche.
- ★ [02-insert-select.kf](solucoes/02-insert-select.kf) — insira e consulte com `db.query` (JSON por linha).
- ★★ [03-query-tipada.kf](solucoes/03-query-tipada.kf) — `record Produto` + `db.query<Produto>`.
- ★★ [04-parametros.kf](solucoes/04-parametros.kf) — `?` com binds (Int/Long/Bool/String).
- ★★★ [05-crud-completo.kf](solucoes/05-crud-completo.kf) — CRUD completo de produtos.

## Transações (aula 03)

- ★★ [06-transacao.kf](solucoes/06-transacao.kf) — commit com 2 inserts dentro de `transaction {}`.
- ★★★ [07-rollback.kf](solucoes/07-rollback.kf) — lance exceção no meio e prove o rollback (`count(*) == 0`).
- ★★★ [08-transferencia.kf](solucoes/08-transferencia.kf) — transferência bancária atômica com validação de saldo.

## CRUD web (aula 04)

- ★★★ [09-crud-web.kf](solucoes/09-crud-web.kf) — API REST `/users` (GET/POST/PUT/DELETE) com `web.app()` + `kof.db` (requer módulo 06).

## Desafio integrador

[10-integrador.kf](solucoes/10-integrador.kf) — biblioteca: tabelas `livros(id, titulo, autor)` e
`emprestimos(id, livro_id, usuario)`. Operações:
1. Listar livros disponíveis.
2. Emprestar (insere em `emprestimos` + marca indisponível) **dentro de `transaction {}`**.
3. Listar emprestados com JOIN.
4. Devolver (remove e marca disponível).

Compila com `kof check`; roda com H2 no classpath.