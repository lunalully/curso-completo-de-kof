# Módulo 03 — Banco de Dados

> **Objetivo:** persistência com `kof.db` — SQL explícito, records tipados e
> transações, sem ORM pesado e sem framework. É a filosofia Kof aplicada a
> dados: *a intenção é a query; o resto é da plataforma.*

## A filosofia

> Acesso a banco é uma capacidade da plataforma. JDBC é o mecanismo interno;
> a API exposta é Kof-idiomática — sem `EntityManager`, `Session`,
> `PersistenceContext` ou `@Transactional`.

```text
intenção → Kof → kof.db → JDBC → banco
```

- SQL-first: você escreve o SQL; records viram bind automático.
- ORM existe para o que é repetitivo (`entity` + CRUD tipado), não como
  dogma — ver `training/idioms/database.md`.
- Gaps de target reportados em compile-time com código (`DB001`, `DB002`,
  `ORM001`, `ORM003`) — nunca fallback silencioso.

## Estado (0.5.0-beta)

| Recurso | Estado |
|---------|--------|
| `db.connect(url)` / `db.connect(url, user, pass)` | ✅ JVM (JDBC), Native (SQLite/MySQL), JS (ponte) |
| `db.execute(handle, sql, args...)` | ✅ JVM, Native x86/cross, JS |
| `db.query(handle, sql, args...)` → `List<String>` (JSON por linha) | ✅ JVM, Native, JS |
| `db.query<T>(handle, sql, args...)` → `List<T>` (records) | ✅ JVM/Native; `DB002` no JS |
| `transaction { ... }` | ✅ JVM, Native; no JS via ponte |
| `db.close(handle)` | ✅ todos (e aviso `MEM014` se esquecer) |
| `orm.*` (CRUD, `where`, page, migrations) | ✅ JVM, Native (SQLite/MySQL); JS fechou `ORM001` |
| `mongodb://` / `oracle://` | MongoDB ✅ JVM/Android (`DB001` nomeado no JS/Native); Oracle via driver |

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-sql.md](01-sql.md) | Fundamentos de SQL (o que você precisa saber) |
| [02-kof-db.md](02-kof-db.md) | A API `kof.db` em detalhe |
| [03-transacoes.md](03-transacoes.md) | `transaction {}`, commit e rollback |
| [04-crud-web.md](04-crud-web.md) | CRUD completo + API REST |

## Pré-requisito

Nenhum setup manual: desde a 0.5.0 o `kof run` provisiona o driver JDBC do
scheme usado (H2, SQLite, PostgreSQL, MySQL/MariaDB, MongoDB) em `~/.kof/deps`.
O driver é resolvido via `ServiceLoader` — sem acoplamento de biblioteca no
runtime Kof.