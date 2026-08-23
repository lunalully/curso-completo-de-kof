# Módulo 09 · Aula 2 — OWASP Top 10 mapeado para Kof

> O OWASP Top 10 é o catálogo das vulnerabilidades mais exploradas em
> aplicações web. Aqui, cada uma mapeada para o que o Kof oferece — e para o
> que você deve implementar.

## A01 — Broken Access Control

**O que é:** usuário acessa recurso sem permissão.

**Em Kof:** middleware + `auth.hasRole`/`hasPermission`.

```kof
app.get("/admin") {
    if (!auth.hasRole("admin")) {
        return "{\"error\": \"forbidden\"}"
    }
    return "{\"ok\": true}"
}
```

## A02 — Cryptographic Failures

**O que é:** dados sensíveis sem criptografia ou com crypto fraca.

**Em Kof:** `kof.security` secure by default — e nunca reinventar.

| Erro | Correto |
|------|---------|
| `sha256(senha)` | `passwords.hash(senha)` |
| senha em texto plano no banco | hash + salt (PBKDF2) |
| token com dados sensíveis | claims mínimas (base64 ≠ cifra) |
| AES-ECB, DES, MD5 | AES-GCM, SHA-256/512, HMAC |

## A03 — Injection (SQLi)

**O que é:** atacante injeta SQL através de entrada não validada.

**Em Kof:** **sempre** `?` placeholders — nunca concatenar SQL.

```kof
// VULNERÁVEL
db.execute(db, "select * from users where name = '" + nome + "'")

// SEGURO — bind
db.query<User>(db, "select * from users where name = ?", nome)
```

## A04 — Insecure Design

**O que é:** falha no *design* (ex.: limite de tentativas de login).

**Em Kof:** defina políticas no design — validação, rate limit (WORKAROUND
manual até a plataforma ter), limites.

## A05 — Security Misconfiguration

**O que é:** default inseguro, headers ausentes, erros verbosos.

**Em Kof:** hardening (aula 05): headers de segurança, erros genéricos,
menos informação vazada.

```kof
// NÃO vaze detalhes de erro para o cliente
catch (String e) {
    log.error("falha interna: " + e)   // detalhe no log
    return "{\"error\": \"erro interno\"}"   // genérico no cliente
}
```

## A06 — Vulnerable and Outdated Components

**O que é:** dependências com falhas conhecidas.

**Em Kof:** a stdlib É a plataforma (menos dependências = menos superfície).
Para drivers JDBC, mantenha atualizados e audite.

## A07 — Identification and Authentication Failures

**O que é:** auth fraca (senha fraca, brute force, sessão exposta).

**Em Kof:** `passwords.hash` (600k iterações), `jwt` com `exp`, verificação
constant-time.

## A08 — Software and Data Integrity Failures

**O que é:** código/dados sem verificação de integridade.

**Em Kof:** HMAC para integridade de dados, assinatura JWT, verificação de
artefatos (`sha256sum -c SHA256SUMS` na distribuição).

## A09 — Security Logging and Monitoring Failures

**O que é:** não logar eventos de segurança → não detectar ataque.

**Em Kof:** `kof.log` para logar logins, falhas e acessos suspeitos.

```kof
app.post("/login") {
    // ... verificação ...
    if (ok) {
        log.info("login ok: " + email)
    } else {
        log.warn("login falhou: " + email + " de " + header("user-agent"))
    }
}
```

## A10 — Server-Side Request Forgery (SSRF)

**O que é:** o servidor faz requests a destinos controlados pelo atacante.

**Em Kof:** como o cliente HTTP é planned, o risco é menor hoje — mas quando
`kof.http` chegar: valide destinos, bloqueie IPs internos, whitelist de
URLs.

## Exercícios

1. Audite a API do módulo 06 contra os 10 itens e anote o que falta.
2. Corrija qualquer SQL concatenado → `?` (se tiver um, crie um).
3. Adicione logging de eventos de segurança (login ok/falhou).
4. Faça um "erro interno" genérico + detalhe no log.