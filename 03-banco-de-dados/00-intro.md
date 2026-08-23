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
- Sem ORM pesado (JPA-style é decisão futura, não pressuposto).
- Gaps de target (Native/JS) reportados em compile-time (`DB001`).

## Estado

| Recurso | Estado |
|---------|--------|
| `db.connect(url)` / `db.connect(url, user, pass)` | ✅ JVM |
| `db.execute(handle, sql, args...)` | ✅ JVM |
| `db.query(handle, sql, args...)` → `List<String>` (JSON por linha) | ✅ JVM |
| `db.query<T>(handle, sql, args...)` → `List<T>` (records) | ✅ JVM |
| `transaction { ... }` | ✅ JVM |
| `db.close(handle)` | ✅ JVM |
| Native / JS | `DB001` (gap em compile-time) |

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-sql.md](01-sql.md) | Fundamentos de SQL (o que você precisa saber) |
| [02-kof-db.md](02-kof-db.md) | A API `kof.db` em detalhe |
| [03-transacoes.md](03-transacoes.md) | `transaction {}`, commit e rollback |
| [04-crud-web.md](04-crud-web.md) | CRUD completo + API REST |

## Pré-requisito

Qualquer driver JDBC no classpath (H2, SQLite, PostgreSQL, MySQL). No JVM o
driver é resolvido via `ServiceLoader` — sem acoplamento de biblioteca no
runtime Kof.