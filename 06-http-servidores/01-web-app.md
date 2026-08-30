# Módulo 06 · Aula 1 — web.app() — Rotas e API

## A API completa

```kof
main() {
    var app = web.app()

    // rotas por método HTTP (lambda trailing)
    app.get("/hello") { return "Hello from Kof" }
    app.post("/echo") { return "got:" + body() }
    app.put("/x")     { return "put" }
    app.delete("/x")  { return "delete" }
    app.patch("/x")   { return "patch" }
    app.options("/x") { return "options" }

    app.listen(8080)
}
```

O corpo `{ ... }` é um **lambda trailing** — o handler da rota. Também pode
ser passado explicitamente: `app.get("/x", handler)`.

## Path params, query, headers

```kof
main() {
    var app = web.app()

    // /users/42?name=mel
    app.get("/users/:id") {
        return "user " + param("id") + " q=" + query("name")
    }

    app.get("/agent") {
        return "agent=" + header("user-agent")
    }

    app.get("/me") {
        return method() + " " + path()
    }

    app.listen(8080)
}
```

| Função de contexto | Retorna |
|--------------------|---------|
| `param("id")` | path parameter |
| `query("name")` | query string |
| `header("x-auth")` | header (case-insensitive) |
| `body()` | corpo cru |
| `method()` | "GET", "POST", ... |
| `path()` | caminho da request |

## JSON de ponta a ponta

```kof
record User(String name, Int age)

main() {
    var app = web.app()

    app.post("/user") {
        var user = json.decode<User>(body())
        return json.encode(user)
    }

    app.listen(8080)
}
```

A resposta detecta JSON automaticamente quando o corpo começa com `{` ou `[`
(`Content-Type: application/json`).

## Semântica de resposta

| Handler retorna | Resultado |
|-----------------|-----------|
| `String` | 200 com o corpo |
| `null` | **404 Not Found** |

### Resposta rica: `status()` + `headerSet()` (0.2.0+)

```kof
main() {
    var app = web.app()

    app.get("/created") {
        return status(201, "criado")   // 201 Created
    }

    app.get("/custom") {
        headerSet("X-Custom", "value")
        return "com header"
    }

    app.get("/json") {
        headerSet("Content-Type", "application/json")
        return status(200, json.encode(User("Mel", 26)))
    }

    app.listen(8080)
}
```

## Servidor

```kof
app.listen(8080)   // bloqueante, 0.0.0.0
app.port()         // porta efetivamente vinculada (útil com listen(0))
app.close()        // encerra (graceful shutdown)
```

## Concorrência

Cada conexão é tratada numa **virtual thread** (JVM). Handlers são síncronos;
o runtime decide a estratégia. O contexto é por-request (ThreadLocal) —
handlers concorrentes não compartilham estado.

## Exercícios

1. Monte uma API com rotas para os 6 métodos e teste cada uma com `curl`.
2. Crie `/saludar/:nome` que responde `"olá, <nome>!"`.
3. Crie um endpoint que recebe um `record` via JSON e devolve de volta.
4. Use `listen(0)` + `app.port()` para iniciar numa porta livre.