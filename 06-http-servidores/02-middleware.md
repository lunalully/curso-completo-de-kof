# Módulo 06 · Aula 2 — Middleware

> Middleware roda **antes** do roteamento. Serve para autenticação, logging,
> validação global — tudo o que toca todas as requests.

## Como funciona

```kof
main() {
    var app = web.app()

    app.use {
        // retorna null → continua para a rota
        // retorna String → resposta imediata (200)
    }

    app.get("/hello") {
        return "Hello from Kof"
    }

    app.listen(8080)
}
```

**Regra de retorno:**
- `return null` → a request segue para o roteamento.
- `return "..."` → resposta imediata; a rota não é chamada.

## Exemplo: autenticação por header

```kof
main() {
    var app = web.app()

    app.use {
        if (header("x-auth") == "secret") {
            return null
        }
        return "{\"error\": \"unauthorized\"}"
    }

    app.get("/hello") {
        return "Hello from Kof"
    }

    app.listen(8080)
}
```

```bash
curl -H "X-Auth: secret" localhost:8080/hello   # 200
curl localhost:8080/hello                        # {"error":"unauthorized"}
```

## Exemplo: logging de requests

```kof
main() {
    var app = web.app()

    app.use {
        log.info(method() + " " + path())
        return null
    }

    app.get("/") {
        return "kof serve online"
    }

    app.listen(8080)
}
```

## Exemplo: API key para rotas específicas

Como o middleware é global, use funções auxiliares para aplicar regras por
rota (uma função que valida e retorna Bool):

```kof
Bool temChaveValida() {
    return header("x-api-key") == secrets.get("API_KEY", "dev")
}

main() {
    var app = web.app()

    app.get("/publico") {
        return "aberto"
    }

    app.get("/privado") {
        if (!temChaveValida()) {
            return "{\"error\": \"forbidden\"}"
        }
        return "{\"dados\": [1, 2, 3]}"
    }

    app.listen(8080)
}
```

## Boa prática

- Middleware para **transversalidade** (auth, logging, rate-limit futuro).
- Rota por rota para **regras específicas**.
- Combine com `kof.security.auth` (módulo 04) para JWT.

## Exercícios

1. Adicione logging de método + path + status em cada request.
2. Proteja todas as rotas com `app.use` + header.
3. Crie `/privado` protegido por API key e `/publico` sem proteção.
4. (Trilha cibersegurança) Como um atacante burlaria o middleware? O que
   falta? (pense em constant-time e em não vazar erros.)