# Módulo 06 — HTTP e Servidores

> **Objetivo:** construir servidores web reais com `kof.web` — rotas,
> middleware, REST e um projeto completo de ponta a ponta. Sem Spring, sem
> servlet container, sem annotations.

## A filosofia

> Uma aplicação web Kof não precisa de Spring. HTTP, rotas, JSON, contexto de
> request e middleware são parte do ecossistema Kof.

```text
intenção → web.app() → rotas → handler (lambda) → String | null
```

## Estado

| Recurso | Estado |
|---------|--------|
| `web.app()` | ✅ JVM |
| Rotas GET/POST/PUT/DELETE/PATCH/OPTIONS | ✅ JVM |
| Path params `/users/:id`, query, headers, body | ✅ JVM |
| Middleware `app.use { ... }` | ✅ JVM |
| `app.listen(port)` / `app.port()` / `app.close()` | ✅ JVM |
| JSON round-trip tipado | ✅ JVM |
| Virtual threads por conexão | ✅ JVM |
| JS (`WEB001`) / Native (sem servidor) | gap compile-time |

## Sumário de aulas

| Aula | Tema |
|------|------|
| [01-web-app.md](01-web-app.md) | Primeiro servidor e API |
| [02-middleware.md](02-middleware.md) | Middleware e autenticação |
| [03-rest.md](03-rest.md) | REST de verdade: recursos, status, JSON |
| [04-projeto-completo.md](04-projeto-completo.md) | API completa: db + security + config + log |

## Primeiro servidor em 10 linhas

```kof
main() {
    var app = web.app()

    app.get("/hello") {
        return "Hello from Kof"
    }

    app.listen(8080)
}
```

```bash
kof serve app.kf          # ou kof run app.kf
curl localhost:8080/hello # Hello from Kof
```