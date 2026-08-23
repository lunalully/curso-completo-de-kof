# Módulo 06 · Aula 3 — REST

> REST é um **estilo**: recursos + verbos HTTP + representação (JSON).
> O Kof deixa isso natural — você modela recursos, o protocolo é HTTP.

## Recursos e verbos

| Recurso | GET (ler) | POST (criar) | PUT (substituir) | DELETE (remover) |
|---------|-----------|--------------|------------------|------------------|
| `/users` | lista | cria | — | — |
| `/users/1` | busca | — | atualiza | remove |

## A API REST de usuários

```kof
record User(Int id, String name)

main() {
    var app = web.app()
    var db = ... // módulo 03

    // listar
    app.get("/users") {
        return json.encode(db.query<User>(db, "select * from users order by id"))
    }

    // buscar por id
    app.get("/users/:id") {
        var users = db.query<User>(db, "select * from users where id = ?", toInt(param("id")))
        if (users.size == 0) {
            return null
        }
        return json.encode(users.get(0))
    }

    // criar
    app.post("/users") {
        var u = json.decode<User>(body())
        db.execute(db, "insert into users values (?, ?)", u.id, u.name)
        return "{\"ok\": true}"
    }

    // atualizar
    app.put("/users/:id") {
        var u = json.decode<User>(body())
        db.execute(db, "update users set name = ? where id = ?", u.name, toInt(param("id")))
        return "{\"ok\": true}"
    }

    // remover
    app.delete("/users/:id") {
        db.execute(db, "delete from users where id = ?", toInt(param("id")))
        return "{\"ok\": true}"
    }

    app.listen(8080)
}
```

## Status codes honestos

A Fase 1 do `kof.web` usa status automáticos:

| Situação | Status |
|----------|--------|
| handler devolve String | 200 OK |
| handler devolve null | 404 Not Found |
| erro interno | 500 |

> **Limitação real:** status customizados (201, 400, 401, 403) ainda não
> existem na Fase 1. Para expressar erro, use o corpo. A trilha de
> cibersegurança discute as implicações (resposta 200 com erro no body é um
> `WORKAROUND` — nunca silencioso, sempre documentado).

## JSON como linguagem do REST

- Request: `Content-Type: application/json` + `body()` + `json.decode<T>`.
- Response: `json.encode(record/list)` + Content-Type automático.

## Convenções REST que você deve seguir

1. Substantivos plurais para recursos (`/users`, não `/getUsers`).
2. Verbo na URL? **Nunca** — use o método HTTP.
3. `:id` nos path params; filtros na query.
4. Stateless: o servidor não guarda sessão — use JWT (módulo 04).

## Exercícios

1. Complete o CRUD com validação de body (nome vazio → erro no body).
2. Adicione `GET /users?q=` filtrando com `like`.
3. Modele um recurso `produtos` completo.
4. Documente sua API (recursos, verbos, exemplos de request/response).