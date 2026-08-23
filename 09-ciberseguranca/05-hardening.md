# Módulo 09 · Aula 5 — Hardening

> Endurecer o servidor: headers de segurança, CSRF, CORS e limitação de taxa.
> O `kof.security` fornece helpers; alguns são `WORKAROUND` manual até a
> plataforma cobrir.

## Headers de segurança

O Kof fornece valores prontos (JVM):

| Helper | Header | Protege contra |
|--------|--------|----------------|
| `security.cspHeader(...)` | `Content-Security-Policy` | XSS |
| `security.hstsHeader()` | `Strict-Transport-Security` | downgrade HTTP |
| `security.contentTypeOptionsHeader()` | `X-Content-Type-Options` | MIME sniffing |
| `security.frameHeader()` | `X-Frame-Options` | clickjacking |
| `security.referrerHeader()` | `Referrer-Policy` | vazamento de referer |

> **Nota real:** a Fase 1 do `web.app()` ainda não permite headers de
> resposta customizados. Use os helpers para **documentar e preparar**, e
> acompanhe o roadmap (G12, TLS). Marque como `WORKAROUND` quando aplicar na
> camada de proxy/frontend.

## CSRF (Cross-Site Request Forgery)

**O que é:** um site malicioso faz o browser da vítima executar ações na sua
app (mudar senha, transferir).

**Em Kof:** tokens CSRF por sessão (JVM):

```kof
main() {
    var app = web.app()

    app.get("/form") {
        var token = security.csrfToken()
        return "{\"csrf\": \"" + token + "\"}"
    }

    app.post("/acao") {
        if (!security.csrfValid(body())) {
            return "{\"error\": \"csrf\"}"
        }
        return "{\"ok\": true}"
    }

    app.listen(8080)
}
```

**Mitigação adicional:** verifique a origem (header `Origin`/`Referer`),
requeira token em estados mutáveis (POST/PUT/DELETE).

## CORS (Cross-Origin Resource Sharing)

**O que é:** browser bloqueia requests entre origens por padrão. CORS libera
**origens específicas** — não `*` para dados sensíveis.

```kof
// helper do kof.security (JVM)
var ok = security.corsAllowed(origin, "https://app.exemplo.com")
```

**Regras:**
- Whitelist de origens; nunca `*` com credenciais.
- `Access-Control-Allow-Origin` ecoa a origem permitida, não a request.
- Preflight (OPTIONS) tratado explicitamente.

## Rate limiting (limitação de taxa)

Protege contra brute force e DoS. **WORKAROUND manual** até a plataforma ter:

```kof
record Tentativa(String chave, Long quando)

class RateLimiter {
    List<Tentativa> tentativas
    Int maximo
    Long janelaMs

    constructor(Int maximo, Long janelaMs) {
        this.maximo = maximo
        this.janelaMs = janelaMs
        tentativas = listOf()
    }

    Bool permite(String chave) {
        var agora = now()
        // remove expirados
        var ativos = listOf<Tentativa>()
        for (var t in tentativas) {
            if (t.chave == chave && agora - t.quando < janelaMs) {
                ativos.add(t)
            }
        }
        tentativas = ativos
        if (tentativas.size >= maximo) {
            return false
        }
        tentativas.add(Tentativa(chave, agora))
        return true
    }
}

main() {
    var limite = RateLimiter(5, 60000)   // 5 por minuto por IP
    var app = web.app()

    app.post("/login") {
        if (!limite.permite(header("x-real-ip"))) {
            return "{\"error\": \"muitas tentativas\"}"
        }
        // ... login real ...
        return "{\"ok\": true}"
    }

    app.listen(8080)
}
```

> `WORKAROUND`: a plataforma ainda não tem `kof.security.ratelimit` nem
> `kof.web` com limiter. O padrão acima é didático e honesto — marque-o.

## Check-List de hardening

- [ ] Headers de segurança (CSP, HSTS, X-Content-Type-Options, X-Frame-Options)
- [ ] CSRF em estados mutáveis
- [ ] CORS com whitelist
- [ ] Rate limit em login e endpoints caros
- [ ] Erros genéricos no cliente + detalhes no log
- [ ] TLS/HTTPS quando disponível (roadmap)
- [ ] Menos informação em respostas (não vaze stack, versões, drivers)

## Exercícios

1. Implemente o `RateLimiter` e teste 6 logins seguidos.
2. Aplique CSRF num POST e valide o token.
3. Monte a checklist de hardening da sua API e marque o que falta.
4. (Reflexão) Por que CSRF importa mesmo com JWT? (pense em requests
   auto-enviadas pelo browser)