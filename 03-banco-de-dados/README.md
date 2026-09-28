# Trilha 03 — Banco de Dados com kof.db

> **Trilha isolada.** Pré-requisito: Trilha 00 (+ 06 para a aula de CRUD web).
> Ao terminar, você modela, persiste, consulta e transaciona com SQL explícito
> e records tipados — sem ORM.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-sql.md](01-sql.md) | SQL: create/insert/select/update/delete |
| [02-kof-db.md](02-kof-db.md) | API `kof.db` (connect/execute/query/query\<T\>) |
| [03-transacoes.md](03-transacoes.md) | `transaction {}` (commit/rollback) |
| [04-crud-web.md](04-crud-web.md) | CRUD + API REST (com módulo 06) |

## Estado e pré-requisito

- `kof.db` roda no **JVM** (JDBC), no **Native** (SQLite e MySQL/MariaDB wire)
  e no **JS** (ponte). Gaps honestos: `DB002` para `query<T>` no JS,
  `ORM001`/`DB001` por recurso.
- Para rodar: nada manual — o `kof run` baixa o driver do scheme (0.5.0).

```bash
kof run app.kf          # driver provisionado automaticamente
```

## Como estudar

1. Leia as aulas na ordem.
2. Faça os [exercícios](exercicios.md) por nível.
3. Confira as [soluções](solucoes/) — compile-as com `kof check`; rode com
   `kof run` (o driver é provisionado).
4. Passe no checkpoint.

## Checkpoint

Construa um CRUD completo de `produtos(id, nome, preco)`:
1. Cria a tabela e insere 3 produtos.
2. `db.query<Produto>` lista, `?` filtra por preço.
3. `transaction {}` atualiza preços atomicamente.
4. DELETE de um produto.

Compila (`kof check`) sem erros → trilha concluída.