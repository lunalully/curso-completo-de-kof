# Módulo 04 · Aula 5 — Autenticação e Autorização Web

> Protegendo APIs com `kof.security.auth` + `web.app()`. Pré-requisito:
> módulo 06 (HTTP).

## Contexto de request

Dentro de handlers/middleware do `web.app()`, o namespace `auth` dá o
contexto do usuário autenticado:

| Função | Retorna |
|--------|---------|
| `auth.secret(secret)` | configura o segredo JWT da aplicação |
| `auth.token()` | o token Bearer da request (String) |
| `auth.authenticated()` | Bool — há token válido |
| `auth.claims()` | claims verificadas (String JSON) |
| `auth.user()` | identificador do usuário (sub) |
| `auth.hasRole("admin")` | Bool |
| `auth.hasPermission("write")` | Bool |

## Autenticação via middleware

```kof
record User(String name, Int age)

main() {
    var app = web.app()
    auth.secret(secrets.get("JWT_SECRET", "dev-secret"))

    // middleware global: tudo que passa por aqui está autenticado
    app.use {
        if (auth.authenticated()) {
            return null            // segue
        }
        return "{\"error\": \"unauthorized\"}"
    }

    app.get("/me") {
        return "usuario: " + auth.user()
    }

    app.get("/admin") {
        if (!auth.hasRole("admin")) {
            return "{\"error\": \"forbidden\"}"
        }
        return "{\"ok\": true}"
    }

    app.listen(8080)
}
```

Teste:

```bash
TOKEN=$(kof run emite_token.kf)   # programa que chama jwt.create
curl -H "Authorization: Bearer $TOKEN" localhost:8080/me
```

## Login (emissão de token)

```kof
main() {
    var app = web.app()

    app.post("/login") {
        // na prática: verifique usuário/senha com passwords.verify
        var u = json.decode<Login>(body())
        if (passwords.verify(u.senha, hashDoUsuario)) {
            var token = jwt.create("{\"sub\":\"" + u.email + "\",\"roles\":[\"admin\"]}", secrets.get("JWT_SECRET", "dev-secret"))
            return "{\"token\": \"" + token + "\"}"
        }
        return null   // 404 — veja a trilha de cibersegurança para status
    }

    app.listen(8080)
}
```

## Comparação `==` vs `constantTimeEquals`

Para comparar tokens, assinaturas ou hashes, **nunca** use `==`:

```kof
if (security.constantTimeEquals(tokenA, tokenB)) {
    // mesma assinatura
}
```

`==` em strings compara byte a byte e **retorna cedo** no primeiro byte
diferente — vaza informação de timing.

## Exercícios

1. Proteja o CRUD de usuários do módulo 03 com `app.use` + `auth`.
2. Adicione uma rota `/admin` restrita a `hasRole("admin")`.
3. Implemente `/login` que devolve um JWT assinado.
4. (Trilha de cibersegurança) Compare uma tentativa com e sem token e observe
   o comportamento do middleware.