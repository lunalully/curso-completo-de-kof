# Módulo 09 · Aula 5 — Hardening

> Endurecer o servidor: headers de segurança, CSRF, CORS, TLS e limitação de
> taxa. Desde a 0.4.1 a plataforma cobre tudo isso — e o `app.security()`
> aplica na ordem certa.

## O jeito curto: `app.security()` (0.4.1+)

```kof
main() {
    var app = web.app()
    app.security()   // rate-limit → CORS → headers → sessão → CSRF → auth → RBAC → rota
    // app.security(mapOf("rateLimit", "10/60", "permitAll", "/health,/login"))
    app.listen(8080)
}
```

- CSRF **ligado por padrão**; `permitAll` libera rotas específicas.
- Headers de hardening aplicados automaticamente (CSP/HSTS/etc. por default
  seguro). JVM; Native/JS = `WEB006`.

O resto da aula mostra as peças por dentro — úteis quando você precisa de
controle fino.

## Headers de segurança

O Kof fornece valores prontos (JVM):

| Helper | Header | Protege contra |
|--------|--------|----------------|
| `security.cspHeader(...)` | `Content-Security-Policy` | XSS |
| `security.hstsHeader()` | `Strict-Transport-Security` | downgrade HTTP |
| `security.contentTypeOptionsHeader()` | `X-Content-Type-Options` | MIME sniffing |
| `security.frameHeader()` | `X-Frame-Options` | clickjacking |
| `security.referrerHeader()` | `Referrer-Policy` | vazamento de referer |

Aplique por resposta com `headerSet(nome, valor)` (0.2.0+):

```kof
app.get("/") {
    headerSet("X-Content-Type-Options", security.contentTypeOptionsHeader())
    return "ok"
}
```

## CSRF (Cross-Site Request Forgery)

**O que é:** um site malicioso faz o browser da vítima executar ações na sua
app (mudar senha, transferir).

**Em Kof:** `app.security()` já liga a validação. Para o fluxo manual
(JVM/JS), emita e valide tokens por sessão:

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

> Gate honesto: `security.csrf*` e os construtores de header são JVM/JS —
> no Native recusam em compile-time (`SECN000`), nunca no-op.

## CORS (Cross-Origin Resource Sharing)

**O que é:** browser bloqueia requests entre origens por padrão. CORS libera
**origens específicas** — não `*` para dados sensíveis.

```kof
// helper do kof.security (JVM)
var ok = security.corsAllowed(origin, "https://app.exemplo.com")
```

`app.security()` aceita `"cors": "<origem permitida>"` para configurar o
middleware. **Regras:**
- Whitelist de origens; nunca `*` com credenciais.
- `Access-Control-Allow-Origin` ecoa a origem permitida, não a request.
- Preflight (OPTIONS) tratado explicitamente.

## Rate limiting — a plataforma tem (0.4.1+)

`security.rateLimit(chave, limite, janelaSegundos)` é real no JVM/JS/Native
x86 (no Native, helper por função):

```kof
app.post("/login") {
    if (!security.rateLimit("ip:" + header("x-real-ip"), 5, 60)) {
        return "{\"error\": \"muitas tentativas\"}"
    }
    return "{\"ok\": true}"
}
```

Ou deixe o `app.security()` cuidar: `"rateLimit": "10/60"` (10 req/60s).

### Por dentro (didático)

Se você quiser entender o algoritmo — janela deslizante em memória — a versão
manual continua válida como exercício:

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
```

> Isso é para **aprender o conceito**; em produção use `security.rateLimit`
> ou `app.security()` — o estado distribuído/multi-instância continua sendo
> decisão de arquitetura sua (ex.: Redis).

## TLS com certificado próprio (0.4.1+)

```kof
app.listenSecure(8443)                            // self-signed de dev (1 arg)
app.listenSecure(8443, certPem, keyPem)           // certificado próprio (PKCS#8 PEM)
```

Em produção atrás de proxy/ingress, o TLS pode terminar antes — decida onde a
intenção "HTTPS" mora. Gate honesto: no Native é `WEB002` em compile-time.

## Check-List de hardening

- [ ] Headers de segurança (CSP, HSTS, X-Content-Type-Options, X-Frame-Options) — `app.security()`
- [ ] CSRF em estados mutáveis — ligado por padrão com `app.security()`
- [ ] CORS com whitelist (nunca `*` com credenciais)
- [ ] Rate limit em login e endpoints caros — `security.rateLimit` ou `"rateLimit": "10/60"`
- [ ] Sessões com cookies seguros — `security.cookieSet` (HttpOnly etc.)
- [ ] Erros genéricos no cliente + detalhes no log
- [ ] TLS/HTTPS (`app.listenSecure`) quando aplicável
- [ ] Segredos em `Secret`/`KeyHandle` (módulo 04) e redigidos no log
- [ ] Menos informação em respostas (não vaze stack, versões, drivers)

## Exercícios

1. Configure `app.security()` com rate limit e permitAll e teste 6 logins
   seguidos.
2. Implemente o `RateLimiter` manual e compare o comportamento com
   `security.rateLimit`.
3. Aplique CSRF num POST e valide o token.
4. Monte a checklist de hardening da sua API e marque o que falta.
5. (Reflexão) Por que CSRF importa mesmo com JWT? (pense em requests
   auto-enviadas pelo browser)
