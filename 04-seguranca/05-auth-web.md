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
| `auth.user()` | identificador do usuário (sub) — ✅ corrigido na 0.4.x (nota #5) |
| `auth.hasRole("admin")` | Bool |
| `auth.hasPermission("write")` | Bool |

## Autenticação via middleware

```kof
main() {
    var app = web.app()
    auth.secret(secrets.get("JWT_SECRET", "dev-secret"))

    // middleware global: tudo que passa por aqui está autenticado
    app.use {
        var logado = auth.authenticated()
        if (!logado) {
            return "{\"error\": \"unauthorized\"}"
        }
        return null            // segue
    }

    app.get("/me") {
        var logado = auth.authenticated()
        if (!logado) {
            return "{\"error\": \"unauthorized\"}"
        }
        var claims = auth.claims()
        return "{\"claims\": " + claims + "}"
    }

    app.get("/admin") {
        var admin = auth.hasRole("admin")
        if (!admin) {
            return "{\"error\": \"forbidden\"}"
        }
        return "{\"ok\": true}"
    }

    app.listen(8080)
}
```

> **Nota:** atribuir antes do `if` (`var admin = auth.hasRole("admin")`) é o
> padrão idiomático do curso. Nos compiladores 0.2.x havia crashes reais aqui
> (`if (!auth.hasRole(...))` direto gerava bytecode inválido; `auth.user()`
> dentro de handler derrubava o servidor) — **corrigidos na 0.4.x**,
> verificados ao vivo nesta trilha. As formas diretas hoje funcionam, mas o
> curso mantém a atribuição por legibilidade. Detalhes em
> [`00-fundamentos/99-notas-workarounds.md`](../00-fundamentos/99-notas-workarounds.md)
> (notas #4 e #5, fechadas).

Teste:

```bash
TOKEN=$(kof run emite_token.kf)   # programa que chama jwt.create
curl -H "Authorization: Bearer $TOKEN" localhost:8080/me
```

## Login (emissão de token)

```kof
record Login(String email, String senha)

main() {
    var app = web.app()

    app.post("/login") {
        // na prática: verifique usuário/senha com passwords.verify
        var u = json.decode<Login>(body())
        if (passwords.verify(u.senha, hashDoUsuario)) {
            var token = jwt.create("{\"sub\":\"" + u.email + "\",\"roles\":[\"admin\"]}",
                                   secrets.get("JWT_SECRET", "dev-secret"))
            return "{\"token\": \"" + token + "\"}"
        }
        return "{\"error\": \"credenciais invalidas\"}"
    }

    app.listen(8080)
}
```

> **Nota:** `return` de variável nua em rota funcionava mal até a 0.2.x
> (nota #5a — o typer não varria `return` aninhado); **corrigido na 0.3.x**
> (bug #28) e verificado nesta trilha. `return null` continua significando
> "sem resposta" → 404 honesto.

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