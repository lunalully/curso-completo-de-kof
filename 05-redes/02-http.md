# Módulo 05 · Aula 2 — HTTP

> HTTP é o protocolo de aplicação da web: **request** e **response**.

## Request

```text
GET /users/42?name=mel HTTP/1.1
Host: localhost:8080
User-Agent: curl/8
X-Auth: secret

<corpo opcional>
```

- **Linha de request**: método, caminho (+ query), versão.
- **Headers**: metadados (`Host`, `Content-Type`, `Authorization`, ...).
- **Corpo**: payload (POST/PUT).

## Métodos

| Método | Intenção |
|--------|----------|
| GET | ler (sem efeito colateral) |
| POST | criar / enviar |
| PUT | substituir |
| PATCH | atualizar parcialmente |
| DELETE | remover |
| OPTIONS | negociar |

## Response

```text
HTTP/1.1 200 OK
Content-Type: application/json

{"ok":true}
```

- **Status code**: `200 OK`, `201 Created`, `400 Bad Request`,
  `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `500 Internal Error`.
- **Headers** e **corpo** como na request.

## HTTP em Kof

O contexto de request está disponível dentro de handlers e middleware:

```kof
main() {
    var app = web.app()

    app.get("/users/:id") {
        return "user " + param("id") + " q=" + query("name")
    }

    app.get("/agent") {
        return "agent=" + header("user-agent")
    }

    app.get("/me") {
        return method() + " " + path()
    }

    app.post("/echo") {
        return "got:" + body()
    }

    app.listen(8080)
}
```

Funções de contexto: `param(name)`, `query(name)`, `header(name)`,
`body()`, `method()`, `path()`.

## Comportamento do servidor

- Handler devolve `String` → 200 com o corpo.
- Handler devolve `null` → **404 Not Found**.
- Corpo começando com `{` ou `[` → `Content-Type: application/json`
  automaticamente.
- Rotas com `:id` → path parameters.

## Testando

```bash
curl localhost:8080/users/42?name=mel
curl -X POST localhost:8080/echo -d '{"a":1}'
curl localhost:8080/nada   # 404
```

## Exercícios

1. Crie rotas para os 6 métodos HTTP e teste com `curl`.
2. Crie `/users/:id` e `/search?q=...` e leia os parâmetros.
3. Retorne `null` numa rota e confirme o 404.
4. Envie JSON num POST e leia com `body()` + `json.decode`.