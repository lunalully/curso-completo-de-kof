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
| `auth.user()` | identificador do usuário (sub) — **quebrado em handler** (nota #5); use `auth.claims()` |
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

> **Nota de versionamento:** os itens #4/#5 das notas de workarounds
> (negação direta de `auth.hasRole` e `auth.user()` em handler) foram
> corrigidos na linha 0.4/0.5, mas não foram re-testados nesta atualização.
> O padrão deste exemplo (atribuir antes do `if`, usar `auth.claims()`)
> é seguro em qualquer versão — se você reproduzir o bug, registre.

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

> **Corrigido na 0.5.0:** `return token;` com variável nua agora registra a
> rota normalmente. Concatenar continua válido quando você monta a resposta.

## `app.security()` — o middleware composto (0.4.1+, JVM)

Em vez de montar rate-limit, CORS, headers, sessão e CSRF à mão, a plataforma
tem um middleware com **ordem fixa** e **CSRF ligado por padrão** (modelo
Spring, `DECISIONS §5`):

```kof
main() {
    var app = web.app()
    auth.secret(secrets.get("JWT_SECRET", "dev-secret"))
    app.security()          // rate-limit → CORS → headers → sessão → CSRF
    // com overrides explícitos (mapa String→String — coleções são homogêneas):
    // app.security(mapOf("csrf", "false", "permitAll", "/health,/login"))
    // app.security(mapOf("rateLimit", "10/60", "headers", "true"))
    app.get("/health") { return "ok" }
    app.listen(8080)
}
```

- CSRF **on por default** quando `app.security()` é configurado
  (`"csrf": false` desliga, com intenção explícita).
- `permitAll` é alias de `publicPaths` (lista CSV) — o modelo Spring.
- Opções: `rateLimit` (Int ou `"10/60"`), `cors`, `headers` (Bool),
  `sessionHeader`, `auth` (Bool) e `roles`.
- Fluxo manual: `security.csrfToken()` / `security.csrfValid(tok)`;
  sessões `security.sessionCreate/Get/Destroy`; rate limit
  `security.rateLimit("ip:" + addr, 10, 60)`.
- Cookies com defaults seguros: `security.cookieSet("sid", sid)` e
  `security.cookieGet` (JVM+JS); headers prontos: `security.cspHeader()`,
  `hstsHeader()`, `frameHeader()`, `contentTypeOptionsHeader()`,
  `referrerHeader()`; `security.corsAllowed(origin, allowed)`.

> **Gates honestos:** `app.security()` é JVM (Native/JS = `WEB006`);
> `security.csrf*` e os construtores de header são JVM/JS (Native =
> `SECN000`). OAuth2 resource server (`auth.resourceServer`, JVM, 0.4.1)
> valida token/escopos no próprio runtime — sem biblioteca de terceiros.

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