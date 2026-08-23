# Módulo 03 · Aula 4 — CRUD + API Web

> Unindo `kof.db` + `kof.web` + `kof.config`: uma API REST completa de
> usuários. Pré-requisito: módulo 06 (HTTP).

## O programa

```kof
record User(Int id, String name)

main() {
    var db = db.connect(config.str("database.url", "jdbc:h2:mem:app"))
    db.execute(db, "create table if not exists users(id int, name varchar(50))")

    var app = web.app()

    // GET /users — listar
    app.get("/users") {
        var users = db.query<User>(db, "select * from users order by id")
        return json.encode(users)
    }

    // GET /users/:id — buscar por id
    app.get("/users/:id") {
        var id = toInt(param("id"))
        var users = db.query<User>(db, "select * from users where id = ?", id)
        if (users.size == 0) {
            return null
        }
        return json.encode(users.get(0))
    }

    // POST /users — criar
    app.post("/users") {
        var u = json.decode<User>(body())
        db.execute(db, "insert into users values (?, ?)", u.id, u.name)
        return "{\"ok\": true}"
    }

    // PUT /users/:id — atualizar
    app.put("/users/:id") {
        var id = toInt(param("id"))
        var u = json.decode<User>(body())
        db.execute(db, "update users set name = ? where id = ?", u.name, id)
        return "{\"ok\": true}"
    }

    // DELETE /users/:id — remover
    app.delete("/users/:id") {
        var id = toInt(param("id"))
        db.execute(db, "delete from users where id = ?", id)
        return "{\"ok\": true}"
    }

    app.listen(config.int("server.port", 8080))
}
```

> **Nota:** a função `toInt(String)` — verifique no seu compilador a forma
> exata de conversão; se não existir, use uma conversão manual simples ou
> mantenha `param("id")` como string no SQL (H2 converte). **Teste sempre.**

## Rodar

```bash
kof build app.kf --target jvm --output out
java -cp "out:h2.jar" Default.Main
```

E teste:

```bash
curl -X POST localhost:8080/users -d '{"id":1,"name":"Mel"}'
curl localhost:8080/users
curl localhost:8080/users/1
```

## Boas práticas já aplicadas

- **Prepared statements** (`?`) em todas as queries — sem SQL concatenado.
- **Records** para bind tipado (`db.query<User>`).
- **Config** da plataforma para URL/porta (`kof.config`).
- **Gaps de target** honestos: `kof.db` e `kof.web` são JVM; Native/JS
  reportam `DB001`/`WEB001` em compile-time.

## Exercícios

1. Adicione um campo `email` ao User e à tabela.
2. Adicione `GET /users?q=nome` que filtra com `where name like ?`.
3. Valide o body antes de inserir (nome não vazio → 400).
4. Proteja o CRUD com autenticação (veja módulo 04 e a trilha de
   cibersegurança, aula de auth web).