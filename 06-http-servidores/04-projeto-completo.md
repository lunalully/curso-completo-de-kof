# Módulo 06 · Aula 4 — Projeto Completo

> A API de ponta a ponta: **db + security + config + log + web**. Tudo da
> plataforma, sem dependência externa além do driver JDBC.

## O projeto: API de usuários autenticada

```kof
record User(Int id, String name, String email)

main() {
    var db = db.connect(config.str("database.url", "jdbc:h2:mem:app"))
    db.execute(db, "create table if not exists users(id int, name varchar(50), email varchar(80))")
    db.execute(db, "insert into users values (1, 'Mel', 'mel@kof.dev')")
    db.execute(db, "insert into users values (2, 'Kof', 'kof@kof.dev')")

    var app = web.app()
    auth.secret(secrets.get("JWT_SECRET", "dev-secret"))

    // logging de todas as requests
    app.use {
        log.info(method() + " " + path())
        return null
    }

    // autenticação global (exceto login)
    app.use {
        var rotaLogin = path() == "/login"
        var logado = auth.authenticated()
        if (!logado && !rotaLogin) {
            return "{\"error\": \"unauthorized\"}"
        }
        return null
    }

    // POST /login → emite token
    app.post("/login") {
        var cred = json.decode<Credencial>(body())
        // na prática: busque o hash no banco e verifique com passwords.verify
        if (cred.email == "mel@kof.dev" && cred.senha == "admin") {
            var token = jwt.create("{\"sub\":\"" + cred.email + "\",\"roles\":[\"admin\"]}",
                                   secrets.get("JWT_SECRET", "dev-secret"))
            return "{\"token\": \"" + token + "\"}"
        }
        return "{\"error\": \"credenciais invalidas\"}"
    }

    // CRUD protegido
    app.get("/users") {
        var users = db.query<User>(db, "select * from users order by id")
        return json.encode(users)
    }

    app.get("/users/:id") {
        var users = db.query<User>(db, "select * from users where id = ?", toInt(param("id")))
        if (users.size == 0) {
            return null
        }
        return json.encode(users.get(0))
    }

    app.post("/users") {
        var u = json.decode<User>(body())
        db.execute(db, "insert into users values (?, ?, ?)", u.id, u.name, u.email)
        return "{\"ok\": true}"
    }

    app.listen(config.int("server.port", 8080))
}
```

> `record Credencial(String email, String senha)` e a função `toInt` precisam
> existir no seu arquivo — verifique o que seu compilador oferece.

## Executando

```bash
# 1. compile
kof build app.kf --target jvm --output out

# 2. execute com o driver JDBC no classpath
java -cp "out:h2.jar" Default.Main

# 3. teste
TOKEN=$(curl -s -X POST localhost:8080/login -d '{"email":"mel@kof.dev","senha":"admin"}' | ...)
curl -H "Authorization: Bearer $TOKEN" localhost:8080/users
```

## O que este projeto ensina

| Camada | Recurso Kof |
|--------|-------------|
| Persistência | `kof.db` (SQL explícito, records) |
| Config | `kof.config` (env > arquivo > default) |
| Logging | `kof.log` + middleware |
| Autenticação | `kof.security` (jwt + auth) |
| Web | `web.app()` (rotas, middleware) |
| Serialização | `json.encode/decode` |

**Sem Spring. Sem annotations. Sem container.**

## Exercícios

1. Adicione validação de email duplicado no POST (consulta antes).
2. Proteja `DELETE /users/:id` com `hasRole("admin")`.
3. Use `kof.log` para logar logins com falha.
4. (Desafio) Implemente "esqueci minha senha" com `passwords.hash` + token
   temporário.